🇺🇸 [English](README.md) | 🇪🇸 [Español](README.es.md)

# REA + VS Code + Cline

![100% free](https://img.shields.io/badge/100%25_free-brightgreen) ![no credit card](https://img.shields.io/badge/no_credit_card-blue) ![open source](https://img.shields.io/badge/open_source-orange) ![4 GB RAM friendly](https://img.shields.io/badge/4_GB_RAM-friendly-yellowgreen)

A free reverse-engineering assistant for low-resource computers.

**What this is:** a step-by-step guide to install REA — an MCP server with 207 reverse-engineering tools — inside VS Code with the Cline agent, powered by Google's free Gemini API. No credit card. No paid subscriptions. Tested on a real low-end laptop (Debian 13, 4 GB RAM, October 2026).

**Who this is for:** anyone curious about how apps work under the hood, even with a modest computer and a zero budget. If you can follow steps one by one, you can do this.

**Why this exists:** powerful AI tools usually assume you own a fast computer or money to spend. This guide proves you need neither: free software, a free API key, and a patient step-by-step path. Not everyone has big resources — and nobody should need them to learn.

**One rule that will save you an hour:** always use a **Flash** model, never a Pro model. The free API key does not allow Pro models.

## What you need

- A computer with Debian/Ubuntu-based Linux (tested on Debian 13)
- 4 GB of RAM (more is fine; less is untested)
- An internet connection
- A Google account (only to get the free API key — no card is ever asked for)

## The stack (verified October 17, 2026)

| Piece | Version | What it does |
|---|---|---|
| VS Code | 1.142.0 | The editor where everything lives |
| Cline | 4.1.25 | The friendly AI agent (a VS Code extension) |
| rea-agents | 6.5.0 | The REA MCP server: 207 reverse-engineering tools |
| Model | gemini-2.5-flash | Google's free, fast model, via a free API key |

**Disk and memory:** VS Code takes about 400 MB; the whole stack about 1.1 GB of disk. RAM rule: run one heavy app at a time and keep the browser closed while you work.

> [!NOTE]
> With 4 GB of RAM, close your browser while using VS Code + Cline. The browser alone can eat the little free memory you have.

## Step 1 — Install VS Code

**What you will achieve:** the code editor installed on your system.

Run this in a terminal:

```bash
wget -O /tmp/vscode.deb "https://code.visualstudio.com/sha/download?build=stable&os=linux-deb-x64" && sudo dpkg -i /tmp/vscode.deb; sudo apt-get install -f -y
```

**What you should see:** lines ending with `Setting up code (1.142.0...)`. Then confirm with:

```bash
which code
```

which should print:

```
/usr/bin/code
```

**Clean up:** the installer file is 229 MB you no longer need. Delete it:

```bash
rm -f /tmp/vscode.deb
```

**How it breaks:** if `dpkg` complains about missing dependencies, the `sudo apt-get install -f -y` at the end fixes it automatically.

## Step 2 — Install Cline

**What you will achieve:** the AI agent added to VS Code.

```bash
code --install-extension saoudrizwan.claude-dev
```

**What you should see:**

```
Extension 'saoudrizwan.claude-dev' v4.1.25 was successfully installed.
```

**How it breaks:** if the install fails, check your internet connection and try again.

## Step 3 — Connect your free Gemini key (Flash, not Pro)

**What you will achieve:** Cline gets a free AI brain.

1. Open VS Code and click the **Cline** icon in the left sidebar.
2. Click the **gear** (settings) at the top of the Cline panel. (Can't find it? Press `Ctrl+Shift+P`, type `Cline`, and open the settings that way.)
3. Set **API Provider** to `Google Gemini`.
4. Get your free key at [aistudio.google.com/apikey](https://aistudio.google.com/apikey) (Google account, no credit card) and paste it into **Gemini API Key**.
5. Set **Model** to `gemini-2.5-flash`.

**What you should see:** at the bottom of the Cline panel it reads `gemini:gemini-2.5-flash`.

**How it breaks:** if you pick a Pro model, every request fails with `400 INVALID_ARGUMENT`. The free key only allows Flash models — switch back to a Flash model and it works.

> [!IMPORTANT]
> Never choose a **Pro** model with the free key. Always **Flash**. This is the single most common error in this setup.

Cline shows prices like `$0.30/M` under the model name. Those numbers are for reference only: without a billing account, Google cannot charge you anything. If you reach the free limit, requests simply pause — you are never billed.

## Step 4 — Plug in REA (the 207 tools)

**What you will achieve:** Cline can now use REA's 207 reverse-engineering tools.

Cline 4.x reads its MCP servers from this exact file:

```
~/.cline/data/settings/cline_mcp_settings.json
```

Write the REA registration into it:

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

**What you should see:** running

```bash
cat ~/.cline/data/settings/cline_mcp_settings.json
```

prints the same JSON you just wrote.

**How it breaks:** older guides point to a different path inside VS Code's storage folders. Cline 4.x ignores that path — if your server never shows up, double-check you wrote the file under `~/.cline/data/settings/`.

> [!WARNING]
> Don't try this with Google Antigravity: it has a hard limit of 100 MCP tools per server, and REA exposes 207. That is exactly why this guide uses Cline instead.

## Step 5 — Reload and verify

**What you will achieve:** proof that REA is alive inside Cline.

1. Press `Ctrl+Shift+P`, type `Reload Window`, press Enter.
2. In the Cline panel, click the **MCP servers** icon (it looks like a stack of servers).
3. You should see **rea** in the list with a green dot (connected).
4. Click **rea** to expand it: it should read **Tools (207)**.

**What you should see:** `Tools (207)`, `Resources (0)`, and the tool list beginning with `annotate_native_function`.

**How it breaks:** if `rea` doesn't appear, re-check the file path from Step 4 and reload the window again.

## Model guide

| Model | Real cost with the free key | Note |
|---|---|---|
| `gemini-2.5-flash` | $0 | Recommended. Fast, and the free tier is generous. |
| `gemini-2.0-flash` | $0 | Older Flash; fine as a fallback. |
| Any `*-pro` model | Not allowed | The free key rejects Pro models (`400 INVALID_ARGUMENT`). |

## Troubleshooting

Stuck? See [docs/troubleshooting.en.md](docs/troubleshooting.en.md): a table of symptom, cause, and fix for the errors people actually hit in this setup.

Prefer a visual guide? Open [docs/index.html](docs/index.html) in your browser.

## License

This guide is shared under [CC BY 4.0](LICENSE): free to use, copy, translate, and adapt — even commercially — as long as you give credit. Free software spirit, as it should be.

## Credits

- REA MCP server: [morluto/rea](https://github.com/morluto/rea)
- Cline agent: [cline/cline](https://github.com/cline/cline)
- Model: Google Gemini via AI Studio (free tier)
