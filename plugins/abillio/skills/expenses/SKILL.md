---
name: expenses
description: Review or edit existing Abillio expenses and follow their submission or approval status; identify receipt-upload steps that require the UI.
---

# Abillio expenses

Read [shared workflow rules](../../references/workflows.md) and
[capability limits](../../references/capability-status.md).

Find expenses with `list_expenses` and inspect `get_expense_endpoint` before acting.
Resolve the intended account, record, date range and currency. Keep draft,
submitted, approved, rejected and paid states distinct; an approval is not proof
of reimbursement. Hidden amounts cannot be used for a total.

For an explicit edit, use `update_expense_endpoint` with the current schema and
read back the result. Do not replace receipt evidence with an invented filename
or URL. New expense creation and receipt upload currently need multipart input
that this MCP does not expose; prepare the requested details and explain the
remaining UI step instead of claiming the expense was created.

For submission, approval or rejection, inspect the existing state and blockers,
confirm the exact target if ambiguous, then use the corresponding available
`submit_expense_endpoint`, `approve_expense_endpoint` or `reject_expense_endpoint`
only when the user requested that transition. Ask for a missing business reason.
A permission denial is not an invitation to switch to an owner account.

`delete_expense_endpoint` applies only to eligible draft/rejected records. A successful
204 response has no body. Verify record state after an ambiguous transport
failure rather than repeating deletion. Do not delete approved
expense evidence to make a report look clean.
