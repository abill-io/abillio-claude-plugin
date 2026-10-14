---
name: freelancers
description: Manage Abillio business–freelancer engagements, review collaboration status or prepare an invitation and delegated invoice.
---

# Abillio freelancers and engagements

Read [shared workflow rules](../../references/workflows.md) and
[capability limits](../../references/capability-status.md). Establish whether the
active account is the business or freelancer side; their available operations differ.

## Engagements and invitations

Use `list_engagements`, `get_engagement` and `list_engagement_services` to establish
the parties, state, accepted services and mandate. The business sends an invitation;
the freelancer's acceptance and mandate are separate human actions.

`create_engagement`, bulk invitation and resend tools send messages. Before sending,
prepare the exact recipients and proposed terms; obtain approval if the user has
not already specified and authorized them. Use `bulk_invite_check` to identify
duplicates and invalid addresses, then inspect each outcome from `bulk_invite_json`.
Retry only failed entries after resolving the cause; never resend a successful batch.

After a timeout or `connection_failed`, immediately reconcile the exact recipient
using `list_engagements` and `get_engagement` before reporting the result. An
invitation may have completed despite a failed response. Do not ask the user to
request this check separately, and never blindly repeat the invitation. If the
read-back also fails, report the outcome as uncertain. Inspect bulk outcomes
individually before retrying any recipient.

Sandbox invitations may simulate acceptance and an active mandate. Report that
as demo behavior, not proof that a person accepted or received an external email.

Use the appropriate update/archive/restore operation for an explicit request.
Describe the effect on the engagement before an unapproved suspension or mandate
revocation. Never accept a commission, contract or mandate for another person.

## Delegated invoicing

The business may prepare a draft for an eligible engagement before mandate
acceptance; issuing requires the accepted, active mandate and current permissions.
Resolve the engagement, issuer and buyer separately.
`create_delegated_invoice_endpoint` must use the selected engagement and an allowed
client; do not silently substitute the business itself for the requested buyer.
For “rēķinu mums”, use the connected Business as buyer only after verifying this
explicit intent and the engagement. Resolve service, VAT and required dates from
the tool and authorized records; ask compactly for missing business inputs.
Follow [invoice preparation](../invoices/SKILL.md), including read-back and retry
handling. Creating the draft does not authorize sending, approval or payment.

When the delegated draft tool advertises `idempotency_key`, generate one UUID
for the intent, keep it with the exact inputs and follow the invoice skill's
bounded same-key retry. The API commits the claim, draft, items, audit and
encrypted answer together. Never use a new key to bypass an uncertain outcome
or conflict. Older servers/tools without that argument still require engagement
invoice reconciliation before considering another creation.
Business self-billing issuance can also approve the received invoice. If the
connection does not expose that action, provide the returned Abillio link for
the human to issue and approve there; never broaden access or claim it was sent.

Keep routine replies to one or two sentences. State the verified outcome or
missing input; omit internal engagement IDs and permission details unless asked.
