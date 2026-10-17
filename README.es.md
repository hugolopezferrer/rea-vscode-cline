🇪🇸 [Español](README.es.md) | 🇺🇸 [English](README.md)

# REA + VS Code + Cline

![100% free](https://img.shields.io/badge/100%25_free-brightgreen) ![no credit card](https://img.shields.io/badge/no_credit_card-blue) ![open source](https://img.shields.io/badge/open_source-orange) ![4 GB RAM friendly](https://img.shields.io/badge/4_GB_RAM-friendly-yellowgreen)

Un asistente gratuito de ingeniería inversa para computadoras de bajos recursos.

**Qué es esto:** una guía paso a paso para instalar REA — un servidor MCP con 207 herramientas de ingeniería inversa — dentro de VS Code con el agente Cline, usando la API gratuita de Gemini de Google. Sin tarjeta. Sin suscripciones de pago. Probado en una laptop modesta de verdad (Debian 13, 4 GB de RAM, octubre de 2026).

**Para quién es:** para cualquiera con curiosidad por saber cómo funcionan las apps por dentro, aunque su computadora sea modesta y su presupuesto sea cero. Si puedes seguir pasos uno por uno, puedes hacerlo.

**Por qué existe:** las herramientas potentes de IA suelen asumir que tienes una computadora rápida o dinero para gastar. Esta guía demuestra que no necesitas ni lo uno ni lo otro: software libre, una API key gratuita y un camino paso a paso con paciencia. No todo el mundo tiene grandes recursos — y nadie debería necesitarlos para aprender.

**Una regla que te ahorrará una hora:** usa siempre un modelo **Flash**, nunca un modelo Pro. La API key gratuita no permite modelos Pro.

## Qué necesitas

- Una computadora con Linux basado en Debian/Ubuntu (probado en Debian 13)
- 4 GB de RAM (más está bien; menos no está probado)
- Conexión a internet
- Una cuenta de Google (solo para obtener la API key gratuita — nunca te piden tarjeta)

## El stack (verificado el 17 de octubre de 2026)

| Pieza | Versión | Qué hace |
|---|---|---|
| VS Code | 1.142.0 | El editor donde vive todo |
| Cline | 4.1.25 | El agente de IA amigable (una extensión de VS Code) |
| rea-agents | 6.5.0 | El servidor MCP de REA: 207 herramientas de ingeniería inversa |
| Modelo | gemini-2.5-flash | El modelo gratuito y rápido de Google, con una API key gratuita |

**Disco y memoria:** VS Code ocupa unos 400 MB; todo el stack, alrededor de 1.1 GB de disco. Regla de RAM: usa una app pesada a la vez y mantén el navegador cerrado mientras trabajas.

> [!NOTE]
> Con 4 GB de RAM, cierra tu navegador mientras usas VS Code + Cline. El navegador solo puede comerse la poca memoria libre que tienes.

## Paso 1 — Instalar VS Code

**Qué vas a lograr:** el editor de código instalado en tu sistema.

Ejecuta esto en una terminal:

```bash
wget -O /tmp/vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64" && sudo dpkg -i /tmp/vscode.deb; sudo apt-get install -f -y
```

**Qué deberías ver:** líneas que terminan en `Setting up code (1.142.0...)`. Luego confirma con:

```bash
which code
```

que debería imprimir:

```
/usr/bin/code
```

**Limpieza:** el archivo instalador pesa 229 MB y ya no lo necesitas. Bórralo:

```bash
rm -f /tmp/vscode.deb
```

**Cómo se rompe:** si `dpkg` se queja de dependencias faltantes, el `sudo apt-get install -f -y` del final lo arregla solo.

## Paso 2 — Instalar Cline

**Qué vas a lograr:** el agente de IA agregado a VS Code.

```bash
code --install-extension saoudrizwan.claude-dev
```

**Qué deberías ver:**

```
Extension 'saoudrizwan.claude-dev' v4.1.25 was successfully installed.
```

**Cómo se rompe:** si la instalación falla, revisa tu conexión a internet e inténtalo de nuevo.

## Paso 3 — Conectar tu key gratuita de Gemini (Flash, no Pro)

**Qué vas a lograr:** Cline obtiene un cerebro de IA gratuito.

1. Abre VS Code y haz clic en el icono de **Cline** en la barra lateral izquierda.
2. Haz clic en el **engranaje** (ajustes) arriba del panel de Cline. (¿No lo encuentras? Pulsa `Ctrl+Shift+P`, escribe `Cline` y abre los ajustes por ahí.)
3. Pon **API Provider** en `Google Gemini`.
4. Obtén tu key gratuita en [aistudio.google.com/apikey](https://aistudio.google.com/apikey) (cuenta de Google, sin tarjeta) y pégala en **Gemini API Key**.
5. Pon **Model** en `gemini-2.5-flash`.

**Qué deberías ver:** abajo del panel de Cline dice `gemini:gemini-2.5-flash`.

**Cómo se rompe:** si eliges un modelo Pro, cada petición falla con `400 INVALID_ARGUMENT`. La key gratuita solo permite modelos Flash — vuelve a un modelo Flash y funciona.

> [!IMPORTANT]
> Nunca elijas un modelo **Pro** con la key gratuita. Siempre **Flash**. Este es el error más común de toda esta instalación.

Cline muestra precios como `$0.30/M` bajo el nombre del modelo. Esos números son solo referenciales: sin una cuenta de facturación, Google no puede cobrarte nada. Si llegas al límite gratuito, las peticiones simplemente se pausan — nunca te facturan.

## Paso 4 — Enchufar REA (las 207 herramientas)

**Qué vas a lograr:** Cline ya puede usar las 207 herramientas de ingeniería inversa de REA.

Cline 4.x lee sus servidores MCP de este archivo exacto:

```
~/.cline/data/settings/cline_mcp_settings.json
```

Escribe el registro de REA en él:

```bash
cat > ~/.cline/data/settings/cline_mcp_settings.json << 'EOF'
{
  "mcpServers": {
    "rea": {
      "command": "npx",
      "args": ["-y", "rea-agents@6.5.0", "mcp"],
      "disabled": false,
      "autoApprove": []
    }
  }
}
EOF
```

**Qué deberías ver:** al ejecutar

```bash
cat ~/.cline/data/settings/cline_mcp_settings.json
```

imprime el mismo JSON que acabas de escribir.

**Cómo se rompe:** las guías viejas apuntan a otra ruta dentro de las carpetas de VS Code. Cline 4.x ignora esa ruta — si tu servidor nunca aparece, revisa que escribiste el archivo bajo `~/.cline/data/settings/`.

> [!WARNING]
> No intentes esto con Google Antigravity: tiene un tope duro de 100 herramientas MCP por servidor, y REA expone 207. Por eso mismo esta guía usa Cline.

## Paso 5 — Recargar y verificar

**Qué vas a lograr:** la prueba de que REA está vivo dentro de Cline.

1. Pulsa `Ctrl+Shift+P`, escribe `Reload Window`, pulsa Enter.
2. En el panel de Cline, haz clic en el icono de **servidores MCP** (parece una pila de servidores).
3. Deberías ver **rea** en la lista con un punto verde (conectado).
4. Haz clic en **rea** para expandirlo: debería decir **Tools (207)**.

**Qué deberías ver:** `Tools (207)`, `Resources (0)`, y la lista de herramientas empezando por `annotate_native_function`.

**Cómo se rompe:** si `rea` no aparece, revisa la ruta del archivo del Paso 4 y recarga la ventana de nuevo.

## Guía de modelos

| Modelo | Costo real con la key gratuita | Nota |
|---|---|---|
| `gemini-2.5-flash` | $0 | Recomendado. Rápido, y el tier gratuito es generoso. |
| `gemini-2.0-flash` | $0 | Flash más viejo; sirve como respaldo. |
| Cualquier modelo `*-pro` | No permitido | La key gratuita rechaza los modelos Pro (`400 INVALID_ARGUMENT`). |

## Troubleshooting

¿Atascado? Mira [docs/troubleshooting.es.md](docs/troubleshooting.es.md): una tabla de síntoma, causa y solución para los errores que de verdad aparecen en esta instalación.

¿Prefieres una guía visual? Abre [docs/index.html](docs/index.html) en tu navegador.

## Licencia

Esta guía se comparte bajo [CC BY 4.0](LICENSE): libre de usar, copiar, traducir y adaptar — incluso comercialmente — siempre que des crédito. Espíritu de software libre, como debe ser.

## Créditos

- Servidor MCP de REA: [morluto/rea](https://github.com/morluto/rea)
- Agente Cline: [cline/cline](https://github.com/cline/cline)
- Modelo: Gemini de Google vía AI Studio (tier gratuito)
