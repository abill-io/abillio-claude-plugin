# Client intake: match the Abillio form

## Required before creating a company client

| Information | Tool field | How to obtain it |
| --- | --- | --- |
| Company or individual | `kind` | Establish from the request; ask if unclear. This plugin only creates `legal` clients. |
| Country of registration | `country` | Ask if not already supplied; use the country's ISO code. |
| Registered company name | `name` | Prefer the company selected through Abillio's registry API. |
| Company registration number | `registration_number` | Registry result or user-supplied company identifier; required even for VIES results. |
| Address | `address_formatted` / `address` | Use the registry address when returned; otherwise ask for the business address. |
| Invoice recipient email | `email` | Ask which address should receive invoices; do not assume a website's general email is the intended recipient. |
| Contact person | `contact_person` | Ask for the person who receives/approves the company's invoices. |
| Phone | `phone` | Ask for the client contact phone if not already supplied. |

Do not submit empty strings, placeholders, invented contacts or guessed addresses
to satisfy these requirements. An optional property in the MCP schema does not
mean the corresponding client form field is optional. These are creation intake
requirements; a partial update should preserve unrelated existing fields.

VAT number is optional. Accounting email (`accounting_email`, copy after payment),
custom display name, internal notes and invoicing preferences are optional too.
Do not turn optional preferences into a questionnaire. Preserve existing/account
defaults unless the user asks to change them. Never derive VAT by prepending `LV`
to a registration number: not every company is VAT registered.

For an individual, direct the user to Abillio's form. Do not collect the personal
code or work around the plugin's legal-entity-only boundary.

## Country-dependent lookup and validation

Use `clients_registry_search`, `clients_registry_vat` and
`clients_registry_vies` for the country-specific checks below. These account-bound
MCP tools use the same Abillio registry functions as the UI. The UI routes below
are context, not permission to make raw HTTP calls.

1. **Latvia:** search Abillio's company register by name or registration number
   (UI: `/api/registry/companies/search`). Resolve ambiguous results with the user.
   Carry the selected registered name, registration number and address forward.
   Check VAT through Abillio's LV VAT lookup (`/api/registry/vat/{number}`); only
   use an active returned VAT number. A successful registry selection corresponds
   to `identity_source=ur_register`.
2. **Other countries supported by the picker’s EU VIES flow:** if the client has
   a VAT number, validate through Abillio's VIES API
   (`/api/registry/vat/vies/{country}/{vat_number}`). Use returned business details
   only on a successful valid result, with `identity_source=eu_vies`. Obtain the
   company registration number separately when missing; VAT is not universally
   interchangeable with it. Missing name/address still needs to be supplied.
3. **Other countries or manual entry:** collect the required business identity
   and contact fields, use `identity_source=manual`, and let Abillio's create API
   apply its country-specific validation and verification. Do not claim manual
   data is verified. A failed lookup is not permission to bypass a rejection.

External searches are useful for finding candidate business details, not proof
of Abillio verification. Never set a registry/VIES identity source solely because
an external page or screenshot contains a match. If a lookup tool is unavailable, report the missing integration concisely. Do
not start browser automation as an automatic fallback or keep searching the web
to work around it. Preserve the collected details so the user can retry after
the connection refreshes. Never claim that a web result was API-verified.

If the country changes, discard the old country's registry/VAT selection and
repeat the appropriate lookup. Distinguish no match, invalid identity and a
temporarily unavailable registry; do not treat all three as verified manual data.

## Conversation and save sequence

- Check existing clients first, including archived matches; do not create a
  duplicate. If a new client's country or company/individual type is unknown,
  ask only for those facts and wait. Do not front-load registration number,
  address and contacts before the country-specific lookup.
- For an LV company, use `clients_registry_search` with its provided **name**.
  Do this before asking the user for a registration number. A number is needed
  from the user only when the search fails, gives no match, or needs
  disambiguation. A brand name alone is not a reason to skip name search.
  For example: “uztaisi klientu Testa Lampa” → ask country/type; “uzņēmums
  Latvijā” → search `Testa Lampa` through Abillio; only then ask for missing
  identity or contacts. Report an unavailable register separately from no match.
- Reuse the verified identity/address and all answers already given. Ask in one
  concise message for only the missing required information. For example, after
  resolving a Latvian company with its address: “Kāds ir rēķinu saņēmēja e-pasts,
  kontaktpersonas vārds un tālrunis?”
- Call `create_client_endpoint` only when the required data is complete. An API
  validation error is not success: use its safe field details to ask for a
  correction. If it returns only a generic error, report that limitation rather
  than retrying guessed payloads or claiming a specific cause.
- Read back with `get_client_endpoint` and report success only after confirming
  the saved client. Do not create/send an invoice as a side effect.

## Maintainer source of truth

Check these together when the client form changes:

- `frontend/apps/dashboard/src/domain/clients/form.ts`: `buildClientFormSchema`
  and `toCreatePayload` (including register-owned address handling).
- `frontend/apps/dashboard/src/shared/ui/forms/company-picker.tsx` and
  `frontend/apps/dashboard/src/shared/core/registry/use-registry.ts`: country
  routing, registry selection and VAT lookup.
- `backend/invoicing/api/schemas/clients.py` and
  `backend/invoicing/operations/clients.py`: accepted input and server validation.

Conversation checks: unknown country asks only country/type; an LV company name
is searched before asking for its registration number; LV registry result with
missing contacts asks all three contact fields; already supplied fields are not
asked again; ambiguous companies require disambiguation; a website VAT claim is
checked through Abillio; no VAT does not block intake; missing registration number
after VIES is requested; a registry error is not presented as verified; individual
clients are handed off to the UI without collecting personal codes.
