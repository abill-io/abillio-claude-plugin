---
name: account
description: Review or update Abillio account profile and invoicing preferences, inspect access and activity, or configure notification preferences within the current connection.
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
