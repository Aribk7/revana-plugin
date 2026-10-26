# Revana plugin for Claude Code

Generate AI images, videos and music from Claude through [Revana](https://revana.si): 120+ models
(Seedance, Veo, Kling, Wan, Hailuo, GPT Image, Nano Banana, Flux, Suno and more) on one API key. Every job
starts on the cheapest provider and falls back automatically; failed generations are free.

## Install

```bash
claude plugin install revana --marketplace Aribk7/revana-plugin
```

Then create an API key at https://revana.si/dashboard and `export REVANA_API_KEY=sr_live_...`. The bundled MCP
server signs in with OAuth the first time you use it (`/mcp`, choose revana).

## What it adds

- **MCP server** `https://revana.si/api/mcp`: list models and prices, free quotes, Seedance 2.5 video and
  Nemesis clip restaging, download links. Works in Claude, ChatGPT, Cursor and Codex too: https://revana.si/mcp
- **Skill** `revana`: Claude calls the REST API for any of the 120+ models, quotes the price, and asks before
  spending.

Prices: https://revana.si/pricing. API reference: https://revana.si/docs.
