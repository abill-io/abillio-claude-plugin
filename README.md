# Abillio for Claude

[Abillio](https://abill.io) is a freelancer management platform: freelancers
and the businesses that work with them use it to issue invoices, manage
clients, record expenses, sign documents and get paid. This plugin connects
Claude to your Abillio account so you can work with invoices, clients,
expenses, freelancers, documents and financial summaries in plain language.
Claude only sees and changes what your Abillio account permissions allow; the
Abillio server checks those permissions on every request.

> **Test environment.** The plugin currently connects to the Abillio test
> environment, not to production. Use it with a test account and test data
> only. Nothing you do through it moves real money or issues real invoices.

The plugin contains no credentials. When you first use it, Claude opens an
Abillio sign-in page in your browser, where you choose which Abillio account
to connect and approve access.

## Install in Claude Code

Add this repository as a plugin marketplace, then install the plugin:

```sh
claude plugin marketplace add abill-io/abillio-claude-plugin
claude plugin install abillio@abillio
```

Or run Claude Code with the plugin folder from a local clone:

```sh
claude --plugin-dir plugins/abillio
```

Then open `/mcp`, select `abillio` and choose Authenticate.

## Install in claude.ai and Claude Desktop

Add the Abillio connector (Customize → Connectors → Add custom connector)
with this server URL, and press Connect:

```text
https://abillio-mcp.abillio.dev/mcp
```

To add the Abillio skills as well, zip the `plugins/abillio` folder and upload
it under Customize → Plugins → Add → Upload plugin. The connector still has to
be connected for the skills to reach your account.

## Support and privacy

- Website: https://abill.io
- Support: https://abill.io/kontakti/
- Privacy policy: https://abill.io/privatuma-politika/

The plugin is proprietary software; see [LICENSE](LICENSE).
