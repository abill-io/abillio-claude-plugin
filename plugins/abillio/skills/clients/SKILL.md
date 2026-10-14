---
name: clients
description: Find, review, create or update Abillio clients, their invoicing defaults and tags, or archive and restore a client.
---

# Abillio clients

Read [shared workflow rules](../../references/workflows.md) and the relevant
[capability limits](../../references/capability-status.md). Use the user's selected
account; similarly named clients in other accounts are separate records.

## Find and review

Use `list_clients` and `get_client_endpoint` to resolve the client. Search by name
and identifying details without assuming a name is unique. Use
`client_selection_summary` for totals; keep its currency groups separate and
respect the distinction between active and archived clients.

## Create or edit

Before creating, read [client intake and validation](references/intake.md).
Establish the client's country and legal/individual kind from the conversation;
ask if missing, never assume the account's country is the client's country.
Ask only for that missing country/type first. For a Latvian company with a name,
call `clients_registry_search` by name before asking the user for a registration
number or address. The register may supply both. Resolve the company identity,
then ask together for the remaining required contacts. Collect the required
form fields before creating, even if the schema marks them optional. Reuse
answers already supplied. External research can find candidate business details;
verification must go through Abillio's API.

Reuse an existing matching client instead of creating a duplicate. Create only
legal-entity clients through this plugin, using business identity and contact
person fields required by the tool. Individual client creation and identity edits
must be completed directly in Abillio. Never request, accept or relay personal
codes, government identifiers, card data or authentication secrets. Do not use
notes or another field to work around a removed input. Do not label manually
entered data as registry-verified.

Use `create_client_endpoint` or `update_client_endpoint` for the requested fields.
Distinguish invoice recipient email, accounting contact and delivery overrides;
ask when changing who will receive future invoices is ambiguous. Omitting an
optional field preserves it; send explicit null only when the API permits clearing.
Use `get_client_endpoint` to verify the result.

## Archive and categorize

`clients_deactivate_client` archives the client without erasing historical invoices;
`clients_activate_client` restores it. A company's own buyer record is protected
and cannot be edited or archived through the normal client workflow.

Resolve an existing tag before using `create_tag` or `api_assign_tag`. Purging a tag
removes all its assignments; it is not a substitute for unassigning it from one
client. Use `api_unassign_tag` for just the selected client assignment.

For a requested invoice, continue with [invoices](../invoices/SKILL.md) after
resolving the client. Client creation alone does not authorize creating or sending one.
