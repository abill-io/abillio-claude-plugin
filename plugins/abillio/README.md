# Abillio for Claude

Abillio is a platform for independent work: freelancers issue invoices and get
paid, businesses engage and pay freelancers, and both sides keep clients,
expenses, documents and payouts in one place. This plugin brings those
workflows into Claude. Ask in plain language — "what is unpaid?", "create a
client", "draft an invoice for Tet, 10 hours at 50 EUR" — and Claude works
with your Abillio account through its tools and skills.

This is a **test-environment** package: it connects to Abillio's shared test
server at `https://abillio-mcp.abillio.dev/mcp`, not to production. Nothing
you do here touches real invoices or money.

## How it connects

Signing in happens in your browser. Claude opens Abillio, you log in, choose
**one** account (Freelancer or Business, demo or real) and approve the access
level for this connection. Claude can never do more than your own current
permissions allow, and you can revoke the connection at any time in Abillio
under Settings → Connected apps. The plugin contains no credentials.

- **claude.ai, Claude Desktop, mobile:** add Abillio as a custom connector
  (Customize → Connectors → Add custom connector, server URL above, or the
  prefilled link in Abillio's Connected apps page) and press Connect.
  Uploading this plugin under Customize → Plugins → Add → Upload plugin adds
  the skills; the connector still has to be connected.
- **Claude Code:** install the plugin from the Abillio marketplace, then run
  `/mcp`, choose `abillio` and select Authenticate:

  ```text
  /plugin install abillio --marketplace abill-io/abillio-claude-plugin
  ```

## What Claude can do

| Skill | Covers |
|---|---|
| setup | Connect, verify the selected account and explain a failed connection |
| invoices | Invoice overview, overdue balances, drafts, explicitly requested sending, reminders, credit notes |
| clients | Clients validated through Abillio's company registry, defaults, archiving |
| expenses | Existing expenses, corrections and status changes |
| freelancers | Freelancer engagements and delegated invoices for businesses |
| documents | Documents, templates, rules and signature statuses |
| money | Balances, ledger, payment status and payouts |
| account | Profile, settings, notifications and activity |

Each connection is fixed to one Abillio account. Business and Freelancer
accounts expose different operations, and a demo (sandbox) account can
simulate payments; a real account cannot. Drafts created from chat are
idempotent and every change is attributed to you in Abillio's activity log.

## What stays in Abillio

Payment execution, personal signatures, file uploads, government identifiers,
credentials and subscription plans are never handled through Claude. When
something needs the Abillio app, Claude says so and links to it.

Interactive invoice and client cards render in claude.ai and Claude Desktop;
Claude Code receives the same record data as text.

## Support

- Website: https://abill.io
- Support: https://abill.io/kontakti/
- Privacy policy: https://abill.io/privatuma-politika/
- Terms: https://abill.io/lietosanas-noteikumi/
