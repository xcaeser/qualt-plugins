# Qualt for Codex and Claude Code

The plugin includes the Qualt workflow skill and connects to
`https://work.elbantli.com/mcp`. Both manifests declare the endpoint. Login uses
OAuth authorization code with PKCE; no API key or secret belongs in the plugin.

## Install

Codex:

```sh
codex plugin marketplace add xcaeser/qualt-plugins
codex plugin add qualt@qualt
```

Claude Code:

```sh
claude plugin marketplace add xcaeser/qualt-plugins
claude plugin install qualt@qualt
```

The public marketplace is [xcaeser/qualt-plugins](https://github.com/xcaeser/qualt-plugins).
It contains only the plugins and installation instructions. The Qualt application
is proprietary and remains in a separate private repository.

For a local checkout, use `plugin marketplace add .` from the marketplace root.

## Sign in

Open the client's MCP connections and choose the Qualt connection's login action.
In Claude Code, use `/mcp`, select Qualt, and choose Authenticate. The browser
opens Qualt. Sign in with your password, passkey, or configured Google/GitHub
account, review the requested access, then allow it. Return to your agent when
sign-in finishes. The client stores and refreshes its own tokens.

Start a new chat and ask “Show my Qualt projects.” Claude Code also exposes
`/qualt:qualt`. The skill instructs agents to read existing work before creating
tasks and to record completion only after verification.

Open **Agent access** in Qualt to see connected agents and disconnect them.
Disconnecting invalidates their tokens and requires a new authorization. If you
deny a request or let it expire, start the login action again in your client.

Remove any old manual Qualt MCP entry to avoid duplicate servers. Remove the
old static Authorization header or bearer-token environment setting from that
entry if you keep it: static credentials can prevent the client starting OAuth.
The plugin does not require `QUALT_API_KEY`.

## Unattended automation

API keys remain available under **Agent access → API keys for automation**.
Create a separate expiring key for each unattended integration and pass it as
`Authorization: Bearer <key>` or `X-Api-Key`. Keys do not authorize account
management. Keep credentials outside Git, skills, chat messages, and task notes.

## Local development

Change the endpoint in both manifests to `http://localhost:3000/mcp` and set
`BETTER_AUTH_URL=http://localhost:3000` in `.dev.vars`. Apply the local migrations
and sign in to the local database. Hosted and local sessions, tokens, and passkeys
are separate. This version requires the OAuth migration and server deployment
before browser login works on the hosted endpoint.
