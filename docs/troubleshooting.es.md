# Troubleshooting

Errores reales de una instalación real (Debian 13, 4 GB de RAM, octubre de 2026). Busca tu síntoma, lee la causa, aplica la solución.

| Síntoma | Causa | Solución |
|---|---|---|
| Cada petición falla con `400 INVALID_ARGUMENT` | Elegiste un modelo Pro. La API key gratuita solo permite modelos Flash. | En los ajustes de Cline, pon **Model** en `gemini-2.5-flash`. |
| `rea` nunca aparece en la lista de servidores MCP de Cline | El config se escribió en la ruta vieja (las carpetas globalStorage de VS Code). Cline 4.x ignora esa ruta. | Escribe el config en `~/.cline/data/settings/cline_mcp_settings.json`, luego `Ctrl+Shift+P` → `Reload Window`. |
| Antigravity muestra "adding this instance with 207 enabled tools would exceed max limit of 100" | Antigravity tiene un tope duro de 100 herramientas MCP por servidor; REA expone 207 y no ofrece filtrado de herramientas. | Usa Cline — carga las 207 herramientas sin tope. (Por eso existe esta guía.) |
| Cline muestra precios como `$0.30/M` bajo el modelo y parece que te van a cobrar | Son precios de lista referenciales que se muestran para cada modelo. | Con la key gratuita y sin cuenta de facturación, Google no puede cobrarte. Si llegas al límite gratuito, las peticiones simplemente se pausan — nunca te facturan. |

¿Sigues atascado? [Abre un issue](../../issues/new/choose) y cuéntanos en qué paso te quedaste.
