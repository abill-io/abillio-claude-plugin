---
name: money
description: Review Abillio balances, ledger activity, outstanding invoices, payouts and financial summaries without treating invoice payment as a completed bank payout.
---

# Abillio money overview

Read [shared workflow rules](../../references/workflows.md) and
[capability limits](../../references/capability-status.md).

Establish the account, reporting dates and requested measure: issued invoices,
outstanding receivables, paid invoices, wallet balance, available funds or payouts.
These are different amounts; do not sum them as if they were independent assets.

Use available balance/wallet tools, `list_ledger_entries`, `list_ledger_operations`
and `get_operation` for the requested record-level explanation. Use
`api_dashboard_summary` and selection-summary tools for scoped aggregates. Preserve
separate totals by currency, the filters used and any incomplete visibility. A 500
or hidden amount is not a zero balance. Different balance endpoints may answer
different measures; do not substitute one for another without explaining it.

For payout status, resolve `list_list_payouts`, `list_get_payout_endpoint` and
`list_payout_history`. Distinguish prepared, approved, submitted, provider-confirmed
and failed states. Quote the actual returned state; payment of an invoice does not
prove a transfer reached the freelancer's bank.

For payment instructions or fee quotes, first resolve the invoice or batch and
its current eligibility. A quote or deposit instruction does not execute payment.
Use [invoices](../invoices/SKILL.md) for receivables and [expenses](../expenses/SKILL.md)
for expense records.

Payment execution and payout approval are outside this MCP's supported authority.
Prepare the explanation and point to the returned Abillio UI link for a requested
payment. Do not call sandbox simulations on a real account or use them as evidence
of a real payment. Bank-detail creation/editing also requires an appropriate
supported UI workflow; never infer or replace a payment destination.

## Simulate invoice payments in sandbox

For an explicit simulation request, verify the connected account has
`is_sandbox=true` and discover its available tools. With `sandbox.simulate`
permission, the supported tools are `post_simulate_paid`, `post_simulate_partial`,
`post_simulate_overpaid` and `post_simulate_chargeback`. Read-only connections
cannot use them. Both Freelancer and Business sandbox accounts remain subject to
their existing invoice visibility and permission rules.

Resolve the invoice and its current status, then use the discovered schema for
fraction, multiplier or reversal reason. Ask for missing inputs, not for approval
already given in an explicit request. Never blindly retry an uncertain mutation:
read the resulting invoice and transaction state first.

After every successful simulation, call `get_invoice` for that same invoice
before answering, so the interactive card shows the new status and received
amount. A card fetched before the simulation is stale. Do not repeat the
simulation to refresh its card. If the fresh read is denied or unavailable,
report the returned simulation status and that the card could not be refreshed.

Report the result as **simulated**, with currency and actual returned status.
These tools create synthetic transactions; they do not transfer real money.
Overpayment can settle the invoice as paid while the excess remains on the bank
transaction for review. Do not promise an overpaid invoice status or a real payout.
Never switch to a paired real account, reset sandbox data, or simulate a payout.
