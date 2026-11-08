# Freshdesk MCP server

Review current tickets and explicitly open a support ticket for an authorized requester.

A direct-provider MCP server with useful local execution. No PipMCP account is required for local use. Provider API access and any provider charges are your responsibility.

## Quick start

Requires Node.js22+. Clone this repository, then run:

```sh
npm ci --ignore-scripts
npm test
npm start
```

The default command inspects the tools offline. Configure your own credentials in your environment:

- `FRESHDESK_API_KEY`
- `FRESHDESK_BASIC_PASSWORD`
- `FRESHDESK_SUBDOMAIN`



Credential help: https://developers.freshdesk.com/api/

## Provider setup

Set your Freshdesk subdomain and personal API key; FRESHDESK_BASIC_PASSWORD must be X. Permission and plan restrictions apply; account-specific mandatory ticket fields may require extending this bounded connector.

## Execute a real read

Replace example record IDs with records you are authorized to access.

```sh
npm start -- --call freshdesk_list_tickets '{"per_page":10,"page":1}'
```

## Use with an MCP client

Launch `node /absolute/path/freshdesk-mcp/server.mjs` using stdio transport; pass credentials through the client environment. Stdout is reserved for JSON-RPC.

## Tools

| Tool | Effect | Purpose |
| --- | --- | --- |
| freshdesk_list_tickets | Read | Read one bounded page of tickets visible to the API key owner. |
| freshdesk_create_ticket | Explicit approval required | Create a ticket; this may trigger configured notifications and automation. |

## Concrete workflow

Review current tickets and explicitly open a support ticket for an authorized requester.

1. freshdesk_list_tickets
2. freshdesk_create_ticket

## Optional writes

Writes are disabled by default. Set CONNECTOR_ALLOW_WRITES=true and supply `_approval: {approved:true,idempotencyKey:"unique-request-id"}` on the specific call. Do not reuse a key for a different request. Uncertain outcomes are never automatically replayed; inspect the provider before another attempt.

## Capability evidence

Local: **Tested — HTTP fixtures and real stdio tools/call**. Managed: **Documented**. Provider authentication and live operations have not been established by these fixture tests. Evidence applies per operation and execution mode; setup discovery does not imply provider execution.

## Managed production

[Import with PipMCP](https://pipmcp.com/github/start?integration=openapi-to-mcp&utm_source=github&utm_campaign=github-freshdesk-mcp&utm_content=readme) when you want the hosted service. This provider is not currently listed as a managed template. Import selected-openapi.json (REST) or the official GraphQL schema linked in provenance, configure authentication, and review the tools before execution. Local credentials are not uploaded by this code. Review provider permissions in the managed onboarding. [Directory and related projects](https://jannikco.github.io/pipmcp-connectors/integrations/freshdesk/).

## Provenance and maintenance

- https://developers.freshdesk.com/api/

Maintainer:jannikco. MIT license for this code. See connector.json for selected operations and integration.json for evidence. Runtime dependencies retain their own licenses. Report sensitive issues through https://pipmcp.com/contact; do not post credentials or customer data in public issues.
