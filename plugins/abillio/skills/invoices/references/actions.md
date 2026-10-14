# Invoice actions

Use this only when the user requests an invoice transition or delivery. Follow
the current tool schema, live v2 decision and actual invoice blockers.

## Send or remind

Inspect the current invoice detail: issuer, client, actual recipient/overrides,
currency, line items, totals, due date, required documents and status. Prepare a
concise preview when those details have not already been approved. A visible,
nonblank invoice `accounting_email` overrides the client email for delivery. Only
fall back to the client email when the invoice override is visible and blank.
If the override is omitted or hidden, do not infer who will receive the invoice;
explain the limitation and use Abillio for that delivery decision. An explicit
instruction to send the identified invoice to its established recipient is
authorization for that send; do not ask again unless a material ambiguity remains.

For an ordinary draft, use `issue_invoice` after resolving blockers. It can
assign a number and enqueue client mail or leave the invoice held for review.
Report the returned status. Issued, queued, sent and delivered are different;
never claim delivery without delivery evidence. Do not issue to preview a PDF.

`send_invoice_endpoint` only records the sent timestamp on an already-issued
invoice; it does not issue drafts or send mail. Never use it as email delivery.
If `issue_invoice` is unavailable, explain that issuing needs Abillio and offer
the prepared draft. Do not substitute another action. A required human
signature must be completed in Abillio. Do not waive it through another route.

For reminders, check that the invoice is still unpaid and eligible, and confirm
the recipient if ambiguous. Use `remind_invoice_endpoint` only for an explicitly
authorized reminder. Read back delivery/invoice status after an uncertain result;
never retry just because the first response timed out.

## Correct or cancel

Edit eligible drafts with `update_invoice_endpoint`. Re-read totals and status
after edits; the backend calculates money, tax and fees. Once confirmed, use the
supported correction workflow instead of pretending a draft edit can rewrite it.

For cancellation, distinguish `cancel_invoice_endpoint` from
`cancel_request_endpoint`, which contacts support and changes reminder behavior.
Prepare the exact invoice and reason before an unapproved cancellation/support
request. Report whether the invoice was cancelled or only a request was submitted.

For a credit note, inspect the original and existing credits, select the requested
line items and quantities, and use `create_credit_note_endpoint` only for a draft
within the original's limits. A credit-note draft does not prove issuance, refund
or a bank transfer. Issuance/payment may require the UI and separate authorization.

## Dates, annotations and files

An expected-payment date is the payer's forecast, not proof of payment. Record one
only when supplied or explicitly approved by the user; do not invent it.

The dedicated private reader-note writer is excluded from MCP until it has a
dedicated writer action. Use the UI for private reader annotations.

PDF/ZIP content must be opened directly in Abillio because documents can contain
restricted identifiers. Do not fetch authenticated URLs through another tool or
send an invoice merely to obtain an attachment.
