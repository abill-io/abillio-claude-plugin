# Capability status — 2026-10-10, repaired local build

These are local results, not proof of hosted deployment or production readiness.
Skills guide workflows; they cannot supply missing API operations or authority.

## Earlier private-build evidence

Native Codex OAuth reads work. Native local calls now successfully delete an
unused template and assign/remove a tag on a disposable client. A real MCP HTTP
→ Django HTTP regression previously covered client creation/deactivation/reactivation,
invoice draft creation/read/edit/deletion, tag assignment/removal, document
creation, invoice/document PDF resources and HTTP 204 success. Authorization
regressions cover field projection, account isolation, ceilings and revocation.
Inbox tests cover sessions, MCP and personal API keys: only the bound human's
inbox is accessible; service keys cannot borrow the account owner's inbox.

## Account-specific discovery — 2026-10-11

The static inventory is not every connection's tool list. Hosted `tools/list`
filters actions through the authenticated v2 preview for the bound account.
Freelancer/Business and sandbox/real are independent axes, exposed explicitly in
identity and discovery. Context-required tools remain discoverable; forbidden,
unavailable and absent actions do not. Direct calls recheck the current preview
and the backend's record-level authorization. No permission or account cache is
shared across HTTP requests. Clients retaining old lists may need to refresh or
reconnect; an old list never grants permission to execute a tool.

Deploy the additive API context fields before or with the MCP server. A new MCP
server talking to an older API refuses discovery rather than guessing the type
or treating an unknown account as a sandbox. Source changes and local tests do
not establish hosted rollout.

## Current workflow limits

Local ChatGPT acceptance on 2026-10-13 reproduced a host catalog mismatch with
both Freelancer and Business connections in one app. Business discovery itself
returned its invitation and delegated draft tools; reconnecting and refreshing
while both connections remained did not restore them in chat. With the user's
approval, retaining only the Business connection and refreshing its tools made
both operations work. Keep API filtering and permissions intact; see setup's
account-change recovery. This does not establish a general host-side fix.

- **Projects:** not released in the customer UI; project tools and new consent grants are
  excluded from this plugin. Freelancer engagements remain supported independently of projects.
- **Private invoice notes:** the dedicated reader-note route is excluded and
  rejects MCP, including full-access tokens. Its current view action cannot
  safely delegate writing. Use the UI until a dedicated writer action exists.
- **Public data boundary:** known government-identifier and credential fields are
  removed from advertised inputs and JSON results. Nested prohibited inputs are
  rejected. Recognizable Latvian/US identifiers, card numbers and credential
  shapes are blocked on input and redacted on output. This is NOT a complete
  classifier for unrestricted free text or health data.
- **Files:** raw PDF/ZIP/image downloads and previews are not exposed; an unexpected
  binary tool result fails closed. Open documents directly in Abillio.
- **Share links:** token-bearing invoice, agreement and member-invitation links
  are omitted from MCP results. Retrieve or share these in Abillio. Authenticated
  UI paths can still be used to open a page that checks the user's own session.
- **Activity details:** payout history omits raw audit metadata. Inbox entries
  expose selected business context, not arbitrary event payloads or rejection
  prose. Open Abillio for details that are not returned; do not infer a reason.
- **Payout detail:** raw provider tax-calculation blobs and operational sync notes
  are omitted; use the returned structured amounts and state for summaries.
- **Bank forms:** provider account/bank requirement forms are omitted together
  with the unsupported payout-method creation workflow.
- **Rendered agreements:** personalized terms HTML stays in Abillio. Document
  signature statuses remain readable; signature material remains unavailable.
- **Clients:** use existing clients or create legal entities. Individual identity
  entry is a UI workflow; never request a personal code in chat.
- **Offers and secrets:** subscription plan catalogs, KYC, notification channels and
  delivery payloads are excluded from discovery.
- **Uploads:** multipart operations remain omitted, including new expenses with
  receipts, receipt upload, payout-method creation/editing and logos.
- **Authority and money:** payment execution, credential/role administration and
  personal signatures/mandate acceptance remain human UI workflows.
- **Business approval queue:** its reader uses the approval action, which this
  MCP ceiling excludes. Unavailable approval counts are not zero; unpaid invoice
  visibility does not authorize an approval or reveal the protected queue.
- **Host discovery:** a host session may expose a subset of server tools. If a
  required tool is unavailable, report that limitation rather than inventing it.

Account kind, record state, plan, verification and account restrictions still
apply. A denial is not an empty result or zero balance. Live provider delivery,
real signatures and production parity have not been verified.

Earlier binary transport evidence describes the private build, not public-plugin availability.

## Sandbox invoice simulation — 2026-10-11

Four explicitly allowlisted POST routes expose invoice full/partial payment,
overpayment and chargeback simulation. Hosted discovery and invocation require
both `is_sandbox=true` and the current `sandbox.simulate` permission preview.
The existing API enforces v2 consent ceilings, invoice visibility and sandbox
status again. Reset, empty, seeding, bulk and payout simulation remain excluded.
This is source/local validation, not evidence that a deployed server is updated.

## Business readiness and delegation — 2026-10-13

New accounts still require the supported authorization-v2 operator cutover
before this MCP connection can discover their tools. A V1 account is an account
readiness blocker, not evidence that the user needs wider permissions. After
cutover, an old consent with an empty fixed ceiling needs a fresh connection
grant; refreshing its tool list cannot expand that ceiling.

Business delegated drafts are supported for eligible engagements. Updated servers
advertise an agent-generated `idempotency_key`: the API atomically commits the
draft and replay answer, and concurrent same-key retries return that authorized
draft. Reuse the same UUID and exact inputs after a timeout. A new intentional
draft needs a new key; old unfinished claims still require reconciliation.
Refresh discovery after rollout; older tools without the key require read-back
before another creation. Dashboard session traffic remains compatible and does
not share API-credential replay records.
Self-billing issuance can approve the received invoice and therefore remains a
human UI step when approval is excluded from the MCP ceiling. Real payments,
bulk-payment execution and personal acceptance remain in Abillio. Demo acceptance
and simulated payment are not evidence of real human consent or money movement.

## Host support differences — 2026-10-14

- **Interactive cards (MCP Apps):** rendered by ChatGPT, claude.ai and Claude
  Desktop. Claude Code and other text-only hosts receive the same record data
  as text; summarize from it and never claim a card was displayed.
- **MCP Events:** available only to hosts negotiating the draft Events
  protocol (ChatGPT). Claude surfaces are not Events clients; do not offer
  subscriptions or automatic notifications there.
- **Skills loading:** in Claude Code the host must allow the `Skill` tool for
  the Abillio workflows to load; without them the server still refuses invalid
  writes and names the rejected fields (`fields` in the refusal), so collect
  those and retry rather than guessing.

