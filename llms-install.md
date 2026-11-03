# Install Aqua Voice MCP in Cline

Aqua Voice is a hosted remote MCP server. This repository supplies client configuration, documentation, and branding; the hosted backend source is not included. Do not build or launch a local server.

## Requirements

- A current Cline version with remote Streamable HTTP and OAuth support.
- An Aqua Voice account. MCP access is available on every Aqua plan.
- Permission to configure MCP servers in your environment.

## Configure the server

In Cline, open **MCP Servers → Remote Servers**. Enter **Aqua Voice**, the URL `https://api.aquavoice.com/mcp`, and **Streamable HTTP**, then select **Add Server**.

Alternatively, add this entry to Cline's MCP settings, preserving all existing servers:

```json
{
  "mcpServers": {
    "aqua-voice": {
      "type": "streamableHttp",
      "url": "https://api.aquavoice.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

For the Cline CLI, use:

```sh
cline mcp add aqua-voice https://api.aquavoice.com/mcp --transport http --yes
```

Use Cline's MCP authorization action to open Aqua's sign-in and consent page. The user must review and authorize the requested access. Aqua supports OAuth discovery, dynamic client registration, and PKCE; no manually copied API key, client secret, or fixed Authorization header is needed. Do not reuse another application's OAuth token.

## Verify

1. Confirm that Cline lists Aqua's tools after authorization.
2. Ask Cline: **Call aqua_get_me to confirm the connected account and whether stored history is available. Do not read transcripts or change settings.**
3. A successful account response verifies the connection. Keep account details and OAuth credentials out of public issue reports.

Optional later uses include searching stored dictations, reading a selected transcript, and managing dictionary entries or supported settings. History availability depends on the user's privacy settings. The MCP does not start microphone recordings or control desktop dictation. Review and approve write operations before they run.

## Troubleshooting

- **401 or authorization required:** use Cline's OAuth authorization action, complete consent, and retry.
- **Transport error:** explicitly select Streamable HTTP; do not use legacy SSE.
- **No stored history:** check `history_available` from `aqua_get_me`; unavailable history cannot be reconstructed.
- **Removal:** disconnect/revoke Aqua authorization and remove the `aqua-voice` entry from Cline's MCP settings. Deleting a configuration entry alone may not revoke the grant.

Documentation: https://aquavoice.com/mcp and https://aquavoice.com/docs/api

Support: support@aquavoice.com
