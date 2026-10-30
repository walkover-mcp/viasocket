<p align="center"><img src="assets/viasocket-logo.png" alt="viaSocket" width="120"></p>

# viaSocket MCP

Lets AI assistants work with [viaSocket](https://viasocket.com): build and manage your plugins (apps) in the Developer Hub, create automations (flows), and connect your product to other apps with the viaSocket SDK.

This is a remote MCP server (Streamable HTTP) at `https://connect.viasocket.com/mcp`. Nothing to install: your AI tool connects to it, and you sign in to viaSocket once through OAuth (the same account as viasocket.com). The docs tools work without signing in.

## Tools
| Area | Tools |
|---|---|
| Docs | `search_viasocket_docs`, `get_viasocket_api_docs`, `quickstart-viasocket-developer-hub` |
| Plugins (Developer Hub) | `list_plugins`, `list_auth_versions`, `list_actions_and_triggers`, `list_action_versions`, `get_plugin_analytics`, `report_published` |
| Account and SDK | `get_org_details_by_token`, `list_embed_projects`, `get_viasocket_embed_secret` |
| Any viaSocket API | `call_any_read_api_of_viasocket` (GET), `call_any_api_of_viasocket` (POST, PUT, PATCH, DELETE) |
| Feedback | `report_feedback` |

## Connect from Cline
Install it from the Cline MCP Marketplace, or open **MCP Servers → Configure MCP Servers** and add this to `cline_mcp_settings.json`:
```json
{
  "mcpServers": {
    "viasocket": {
      "type": "streamableHttp",
      "url": "https://connect.viasocket.com/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```
Then use the sign-in button next to `viasocket` in the MCP Servers list and sign in to viaSocket in the browser. Agent-driven install steps are in [llms-install.md](llms-install.md).

## Connect from Claude Code
```bash
claude mcp add --transport http viasocket https://connect.viasocket.com/mcp
```
then run `/mcp` and sign in.

## MCP Registry
[server.json](server.json) is the listing for the official MCP Registry, as `com.viasocket/mcp`. The `com.viasocket` name is proven by owning the `viasocket.com` domain.

One-time setup: create a signing key and publish its public half on `viasocket.com` (Git Bash or any shell with OpenSSL):
```bash
openssl genpkey -algorithm Ed25519 -out mcp-registry-key.pem
echo "v=MCPv1; k=ed25519; p=$(openssl pkey -in mcp-registry-key.pem -pubout -outform DER | tail -c 32 | base64)"
```
Add the printed line as a TXT record on `viasocket.com`. Keep `mcp-registry-key.pem` private and out of this repo.

Publish (bump `version` first for any later change):
```bash
mcp-publisher login dns --domain viasocket.com --private-key "$(openssl pkey -in mcp-registry-key.pem -noout -text | grep -A3 'priv:' | tail -n +2 | tr -d ' :\n')"
mcp-publisher publish
```

## Links
- Website: https://viasocket.com
