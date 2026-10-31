# Revana plugin for Claude Code

Generate AI images, videos and music from Claude through [Revana](https://revana.si): 120+ models
(Seedance, Veo, Kling, Wan, Hailuo, GPT Image, Nano Banana, Flux, Suno and more) on one API key. Every job
starts on the cheapest provider and falls back automatically; failed generations are free.

## Install

```bash
claude plugin install revana --marketplace Aribk7/revana-plugin
```

The bundled MCP server signs in to your Revana account with OAuth the first time you use it (`/mcp`, choose
revana). No API key is needed.

## What it adds

- **MCP server** `https://revana.si/api/mcp`: list models and prices, free quotes, Seedance 2.5 video and
  Nemesis clip restaging, download links. Works in Claude, ChatGPT, Cursor and Codex too: https://revana.si/mcp
- **Skill** `revana`: Claude prices a video with a free quote, asks before spending, makes it with Seedance 2.5 and
  returns the download link. For the other 200+ models over REST, see https://revana.si/docs.

Prices: https://revana.si/pricing. API reference: https://revana.si/docs.
