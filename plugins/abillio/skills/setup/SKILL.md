---
name: setup
description: Connect the Abillio test environment in Claude (claude.ai, Claude Desktop or Claude Code) with browser sign-in, or explain a failed connection in plain language.
---

# Connect Abillio in Claude

This private plugin connects to the Abillio **test environment** at
`https://abillio-mcp.abillio.dev/mcp`. Claude handles OAuth sign-in; the
plugin contains no credentials. Use this workflow when the user wants to
connect, or when an Abillio tool reports that authentication is required.

## How the connection is made on each Claude surface

- **claude.ai, Claude Desktop, mobile:** Abillio is a custom connector. The
  user adds it once under Customize → Connectors (or opens Abillio's prefilled
  "Add custom connector" link from Settings → Connected apps in Abillio) and
  presses **Connect**. In a Team/Enterprise organization an owner adds the
  connector under Organization settings → Connectors; each member then
  connects their own Abillio account. If this plugin was uploaded in claude.ai,
  its Connectors tab lists Abillio; the user still has to connect it there.
- **Claude Code:** the plugin declares the `abillio` HTTP MCP server. The user
  runs `/mcp`, selects `abillio` and chooses **Authenticate**; the terminal
  opens the browser. Without the plugin, the equivalent is
  `claude mcp add --transport http abillio https://abillio-mcp.abillio.dev/mcp`
  followed by `/mcp` (or `claude mcp login abillio`). `claude mcp logout abillio`
  clears the stored grant.

This is browser sign-in, not a local development task. Do not ask for
filesystem access, local files, API keys, a `connection.json`, a private key,
a password or a token in chat, and do not run shell commands on the user's
behalf to "install" the connection. No Python or Abillio source checkout is
required. Keep routine onboarding explanations short.

## Customer connection flow

1. If Abillio account tools already work, the connection is ready: identify the
   account and continue the user's task without repeating setup.
2. Otherwise tell the user this is Abillio's test environment and point them to
   the Connect / Authenticate action of the surface they are on (above). The
   browser opens Abillio login, then account selection and consent. Request only
   the access needed for the task. Never silently grant writes, switch
   environments, use a staff connector or broaden permissions.
3. After authorization, discover the actual tools and verify the selected
   account with the session/account tools (`whoami`, `connection_profile`,
   `authorization_preview`). Each connection authorizes exactly one account.
   Show its name, its kind (Freelancer or Business) and whether it is a sandbox,
   plus the test-environment label. To use another account, the user reconnects
   through Claude and chooses that account; never switch it through a tool. If
   discovery returns multiple accounts, stop and report an integration defect.
   A completed browser login or MCP handshake alone does not prove invoice access.
4. If authentication fails, explain the concrete blocker and the next user
   action, if any, in plain language. Do not dump configuration instructions.
   If the server exposes generic `kwargs` instead of parameter fields, or cannot
   identify the account, this is an integration defect for the developer to
   fix; repeatedly asking the user to reconnect will not fix it.

The server owns authorization on every call. On denied access, stop that action
and explain it; do not bypass the grant. A refusal is a decision about this
connection's grant, not missing data: never retry the action through another
Abillio connection or server (even one for the same account) and never ask
the user to widen access just to finish; tell them which access level or
Abillio action the step needs and stop. Deployment diagnosis belongs to a
separate developer task, not customer sign-in. Do not claim the test plugin is a
public or production release.

## Missing host connection capability

Do not invent a Setup, Connect account or Sign in button, menu path or callable
authentication tool beyond the surfaces described above. If no Abillio tools
are exposed in this conversation, report that this Claude surface has not loaded
the Abillio connection (the connector was not added or not connected, or in
Claude Code the server is not configured or not authenticated); do not send
the user through guessed UI steps. A skill cannot implement OAuth by inventing an
authorization URL: Claude must create the request, PKCE state and callback and
retain the resulting credentials.

## Account tools missing after an account change

Claude keeps the tool list of the connection it has. After the user reconnects
with a different account (Freelancer ↔ Business), the previous catalog can
linger until the connector is reconnected or, in Claude Code, `/mcp` reconnects
the server. If identity tools work but an expected Business/Freelancer
operation is missing, explain that the connection's tools need refreshing; do
not claim that the user lacks permissions. Use only a refresh, reconnect or
account-selection control actually available on the surface. Do not remove
another connection without the user's approval. Reconcile uncertain writes
before any retry, especially after reconnecting changes the credential.

## Account type and mode

Read the server-returned account `kind` and `is_sandbox` before choosing a
workflow. `company` means Business; `freelancer` means Freelancer. Each type may
be Sandbox/demo (`is_sandbox=true`) or a real account (`false`). Name both axes
alongside the account name. A real account on a test deployment is not a sandbox;
the deployment environment and the account mode are independent.

Use only tools actually discovered for this connection. Business and Freelancer
workflows differ; do not infer capabilities from account names, ownership or a
sandbox label. Context-required actions still need their record-specific checks.
Sandbox never expands permissions, authorizes arbitrary test writes, or promises
that every external effect is simulated. Its paired real account requires its
own authorization; never follow a sandbox-pair ID to switch accounts.

If type/mode cannot be determined, stop business actions and explain the missing
account context. Reconnect for a different account. Never reuse a previous
connection's cached tool list as authority.

## Interactive cards

On claude.ai and Claude Desktop, invoice and client tools may render an
interactive card (MCP App). Let the card carry the details and add at most one
or two sentences. Pass the card tools' optional `language` argument (`lv` or
`en`, the language the user writes in) so the card and the messages its
buttons prefill match the conversation. A card button only prefills a message
the user then sends; treat it as the user's request. Claude Code does not
render cards: the same record data is returned as text in the tool result, so
summarize from it and never claim a card was shown.
