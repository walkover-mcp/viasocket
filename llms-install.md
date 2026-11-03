# Installing the viaSocket MCP server

These steps are for AI agents (such as Cline) setting up this server for a user.

viaSocket MCP is a remote server. There is nothing to clone, build or run locally: the client connects to `https://connect.viasocket.com/mcp` over Streamable HTTP. There is no API key to ask for; the user signs in to viaSocket through OAuth in their browser.

## 1. Add the server to the MCP settings
Add this entry to the `mcpServers` object in the MCP settings file (for Cline: `cline_mcp_settings.json`). Keep any servers already in the file.

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

The `type` must be `streamableHttp`.

## 2. Sign in
The docs tools work right away. Tools that use the user's apps or change their viaSocket account (app actions, flows, plugins, embed projects) need the user to sign in: in Cline's **MCP Servers** list, use the sign-in button next to `viasocket`. A browser window opens for the viaSocket sign-in (the same account as https://viasocket.com). Ask the user to finish it; do not try to sign in for them.

## 3. Verify
Call `search_viasocket_docs` with a question such as "how do I add an action to my plugin". It should return passages from the viaSocket docs. After sign-in, `get_org_details_by_token` returns the user's viaSocket organization.

## Using it
To use the user's apps (Gmail, Slack, Google Sheets, HubSpot and more), read https://viasocket.com/docs/skill.md first and follow it. For plugin and Developer Hub work, call `quickstart-viasocket-developer-hub` first: it loads the instructions for those tools. Confirm with the user before creating, publishing, deleting or running anything.

## Tools
- Docs: `search_viasocket_docs`, `get_viasocket_api_docs`, `quickstart-viasocket-developer-hub`
- Your apps (Gmail, Slack, Google Sheets, HubSpot, 2,300+ more): `search_apps`, `list_app_actions`, `list_connections`, `create_connection_link`, `run_app_action`
- Plugins (Developer Hub): `list_plugins`, `list_auth_versions`, `list_actions_and_triggers`, `list_action_versions`, `get_plugin_analytics`, `report_published`
- Account and SDK: `get_org_details_by_token`, `list_embed_projects`, `get_viasocket_embed_secret`
- Any viaSocket API: `call_any_read_api_of_viasocket` (GET), `call_any_api_of_viasocket` (POST, PUT, PATCH, DELETE)
- `report_feedback`: report an error or wrong docs to viaSocket
