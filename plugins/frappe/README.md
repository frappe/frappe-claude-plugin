# Frappe plugin for Claude Code

Connect Claude Code to your Frappe or ERPNext site. Ask Claude to find records, make reports, and create or change documents. Claude acts as you, with your roles and permissions.

The plugin uses the MCP server that is built into every Frappe site at `/api/v2/mcp`. It works with any site address: self-hosted, Frappe Cloud or a custom domain.

## Install

```
/plugin marketplace add frappe/claude-plugins
/plugin install frappe@frappe
```

Claude Code asks for your site URL, for example `https://erp.example.com`. Then run `/mcp`, select `frappe` and log in to your site in the browser. Approve the access request.

To change the site later:

```
claude plugin configure frappe@frappe
```

## Requirements

- A Frappe site on a version that has the built-in MCP server.
- A desk user (System User) account on that site.

## Troubleshooting

New sites have MCP and OAuth discovery turned on. If the connection fails on an older or customized site, ask your administrator to check:

- **System Settings**: "Enable MCP" is on.
- **OAuth Settings**: authorization server metadata, resource metadata and dynamic client registration are on.

## More than one site

The plugin connects to one site. To add another site, run:

```
claude mcp add --transport http frappe-staging https://staging.example.com/api/v2/mcp
```

## What is included

- MCP server `frappe`: search DocTypes and methods, read, count, create, update and delete documents, and call whitelisted methods.
