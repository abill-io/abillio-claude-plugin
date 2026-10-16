---
name: account
description: Review or update Abillio account profile and invoicing preferences, see what the account still has to provide, inspect access and activity, or configure notification preferences within the current connection.
---

# Abillio account settings and activity

Read [shared workflow rules](../../references/workflows.md) and
[capability limits](../../references/capability-status.md).

Use `get_profile`, `get_account_settings` and `get_invoice_defaults_endpoint` to
read current values before a requested settings change. Patch only the requested
fields using the available profile/settings/defaults tools. Clarify the intended
recipient before changing invoice-delivery defaults. Read back the result; an
account settings edit does not rewrite historical invoice snapshots.

`get_profile` can provision an empty billing profile if one is missing.
`get_preferences` can initialize default notification channels and subscriptions
if the email setup is missing. Explain these setup effects when relevant; these
tools are not guaranteed to be free of writes merely because they retrieve data.

For access questions, explain the user's live v2 rights intersected with the
connection ceiling using `authorization_preview`. Full access never means rights
the user lacks. Use available member/role readers to explain returned
information, but do not treat them as permission to invite users, assign roles,
mint keys or widen grants. Those authority workflows remain in the UI.

Use `api_activity_feed`, `audit_audit_log` and access-log tools for requested
activity/date filters. Describe observed events without inferring intent or
attributing a system action to a human unless the record supports it.

For notifications, inspect preferences and the current human inbox with
`get_preferences` and `get_inbox`. Inbox/count tools use the bound human's inbox;
service keys have no personal inbox. A denied request is not an empty inbox.
Channel configuration and delivery payloads remain in Abillio because they can
contain authentication secrets or arbitrary message content.

Connection revocation, verification, personal signing and account closure remain
human UI workflows. Do not display subscription offers or plans, promote upgrades,
or start subscription checkout. You may explain the current account entitlement
when a feature is unavailable, without marketing another plan.

## Requirements

`get_requirements` lists what a Freelancer account still has to provide (identity
verification, a payout method, documents to sign and so on), each with the `link`
to the Abillio page where it is completed. Call it when the user asks what is left
to set up, and when a write is refused with `kyc_missing`, `payout_method_missing`,
`documents_unsigned`, `contract_requirement_pending`, `mandate_missing` or
`mandate_revoked`; do not retry the refused write. List each `due` item in the
user's language with its link, naming the company when `company_name` is set,
and mention each `upcoming` item with its `due_at` date.

`payout_method_missing` on issuing means this draft has no payout method: if the
list has no `payout_method` item, attach the user's chosen verified method to the
draft (`list_payout_methods` → `payout_method_id`) or let them choose it in
Abillio, then issue again. If no item matches the refusal, say what it means and
that it is resolved in Abillio.

Identity verification, document signatures, the mandate and the member fee are
the user's to complete in Abillio: never invent a workaround or complete them for
the user. Billing details are different: a missing address can be filled in here
with `accounts_patch_profile` when the user gives it, like any requested profile
change above.

A Business connection has no `get_requirements`: a freelancer's list is the
`requirements` of their engagement in `list_engagements` or `get_engagement`.
