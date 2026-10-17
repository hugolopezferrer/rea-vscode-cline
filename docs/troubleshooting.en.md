# Troubleshooting

Real errors from a real installation (Debian 13, 4 GB RAM, October 2026). Find your symptom, read the cause, apply the fix.

| Symptom | Cause | Fix |
|---|---|---|
| Every request fails with `400 INVALID_ARGUMENT` | You selected a Pro model. The free API key only allows Flash models. | In Cline settings, set **Model** to `gemini-2.5-flash`. |
| `rea` never appears in Cline's MCP server list | The config was written to the old path (VS Code's globalStorage folders). Cline 4.x ignores that path. | Write the config to `~/.cline/data/settings/cline_mcp_settings.json`, then `Ctrl+Shift+P` → `Reload Window`. |
| Antigravity shows "adding this instance with 207 enabled tools would exceed max limit of 100" | Antigravity has a hard cap of 100 MCP tools per server; REA exposes 207 and offers no tool filtering. | Use Cline instead — it loads all 207 tools with no cap. (This is why this guide exists.) |
| Cline shows prices like `$0.30/M` under the model name and it looks like you'll be charged | Those are reference list prices shown for every model. | With the free key and no billing account, Google cannot charge you. If you hit the free limit, requests simply pause — you are never billed. |

Still stuck? [Open an issue](../../issues/new/choose) and tell us which step stopped you.
