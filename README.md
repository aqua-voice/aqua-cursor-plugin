# Aqua Voice for Cursor

Connect Cursor and Grok Bot to your Aqua Voice account. Search stored dictations, read transcripts, and manage your Aqua preferences through the remote MCP server.

## What you can do

- Search your stored Aqua sessions and read their transcripts.
- Read and update your dictionary, text replacements, and custom instructions.
- Read and change supported settings, including language.
- Send feedback or a correction for a selected dictation when you request it.

History availability depends on your Aqua privacy settings. The connection accesses only the Aqua account you authorize. It does not start microphone recordings or control desktop dictation. Feedback does not replace text already inserted in another app.

## Requirements

- A current version of Cursor with plugin support.
- An Aqua Voice account. MCP access is available on every Aqua plan.
- Permission to install plugins in your Cursor workspace, if managed by an organization.

This plugin is free and open source. Aqua account billing is handled separately; the plugin has no purchase or upgrade tools.

## Install locally

To use an existing checkout, open **Customize → Add → From Local Repository** in Cursor and select this repository's directory. The included marketplace manifest exposes the Aqua Voice plugin for installation.

Until the marketplace listing is approved, clone this repository into Cursor's local plugin directory:

```sh
mkdir -p ~/.cursor/plugins/local
git clone https://github.com/aqua-voice/aqua-cursor-plugin.git ~/.cursor/plugins/local/aqua-voice
```

Restart Cursor or run **Developer: Reload Window**. Open **Customize** and confirm that the Aqua Voice plugin and its `aqua-voice` MCP server appear. Enterprise workspaces may require an administrator to enable local plugin imports. Copy the directory itself when testing; Cursor does not load local-plugin symlinks whose targets are outside the local plugin directory.

## Connect your account

1. Open the Aqua MCP server in Cursor's MCP controls and choose its sign-in or authentication action.
2. Sign in to Aqua and review the requested permissions. Your client may request read and write access together.
3. Ask: **"Call aqua_get_me and show which Aqua account is connected and whether stored history is available."**

The plugin connects to `https://api.aquavoice.com/mcp` using Streamable HTTP. OAuth discovery, dynamic client registration, and PKCE handle sign-in. No API key, client secret, local server, or package installation is needed. If a write requires additional consent, complete the authorization prompt before retrying.

## Try it

- "Find my recent Aqua dictations about the release plan."
- "Summarize the transcript of the dictation I selected."
- "Show my Aqua dictionary and text replacements."
- "Add this project's name to my Aqua dictionary."
- "Set my Aqua language to French."

The last two examples change your Aqua account. Review the proposed changes before approving tool calls.

## Troubleshooting and removal

- **No plugin appears:** reload Cursor and check that `.cursor-plugin/plugin.json` is inside `~/.cursor/plugins/local/aqua-voice`. A marketplace installation with the same name takes precedence over the local copy.
- **Authentication is required:** reconnect the Aqua MCP server and complete the browser sign-in and consent flow.
- **No stored transcripts:** check `history_available` returned by `aqua_get_me` and your Aqua privacy settings. Unavailable history cannot be reconstructed by this plugin.
- **A write is denied:** inspect the permissions granted to the connection and reauthorize if prompted.
- **Remove the plugin:** disconnect/clear the Aqua server's authentication in Cursor, then uninstall the plugin or remove its local plugin directory. Removing files alone does not necessarily clear saved OAuth credentials.

## Links

- [Aqua MCP](https://aquavoice.com/mcp)
- [Tool reference and connection documentation](https://aquavoice.com/docs/api)
- [Privacy policy](https://aquavoice.com/info/privacy)
- [Aqua terms](https://aquavoice.com/info/terms)
- [Support](mailto:support@aquavoice.com)
- [Cursor plugin documentation](https://cursor.com/docs/plugins)

## MCP Registry listing

`server.json` describes the hosted Aqua Voice MCP server for the official MCP Registry, under `io.github.aqua-voice/aqua-voice`. This repository contains the client configuration and listing assets; it does not contain the hosted backend source code.

Maintainers can publish the listing by running **Publish to MCP Registry** from this repository's GitHub Actions page on `main`. The workflow uses GitHub OIDC and does not require a stored registry token. Update the version in `server.json` before publishing a new listing version. Registry publication and Cursor Marketplace review are separate processes.

## License

The plugin package is available under the [MIT License](LICENSE). Aqua Voice names and logos remain trademarks of their respective owners; the license does not grant trademark rights. The hosted Aqua service is governed by Aqua's terms.
