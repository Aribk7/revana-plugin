---
name: revana
description: Make AI videos with Seedance 2.5 and restage clips with Nemesis through the Revana MCP server (revana.si), with a free price quote before anything is spent. Use when the user asks to make, render or generate a video, clip or animation with AI, wants the cheapest Seedance API, or asks what Revana's models cost.
---

# Revana: AI video from Claude

Revana (https://revana.si) runs image, video and music models behind one account. Each job starts on the cheapest
provider and falls back automatically; only a generation that succeeds is charged, and a failed one costs nothing.
Credits are prepaid, 100 credits = $1.

This plugin works through the bundled **revana MCP server** (`https://revana.si/api/mcp`). The first time a Revana
tool is used, the user signs in to Revana with OAuth (in Claude Code: `/mcp`, then choose revana). Never ask the
user to paste an API key into the chat.

## Tools

| Tool | Use it to |
|---|---|
| `list_models` | List models with their options and live prices |
| `get_balance` | See how many credits the account can spend |
| `get_quote` | Price a Seedance 2.5 video before making it (free) |
| `generate_video` | Make the video (spends credits) |
| `get_video` / `get_video_link` | Check progress and get a download link |
| `upload_file` | Upload a reference image or clip the user provides |
| `quote_nemesis` / `make_nemesis` | Price and run a Nemesis restage of an existing clip |

## How to work

1. Pick settings with the user: prompt, resolution (480p, 720p or 1080p), duration in seconds, aspect ratio,
   and whether to add audio. If they give a start frame or reference, upload it with `upload_file` first.
2. Call `get_quote`, then **tell the user the exact price and wait for a clear yes** before calling
   `generate_video` or `make_nemesis`. Check `get_balance` if the quote may exceed what is left.
3. After `generate_video`, poll `get_video` every 10-20 seconds; video usually takes one to several minutes.
4. When it succeeds, give the user the link from `get_video_link`. If it fails, report the error code; failed
   generations are not charged, and a `retryable` error can simply be tried again.

`buy_credits` charges the card the user saved at revana.si. Only call it when the user explicitly asks to buy a
specific amount of credits, and confirm the amount first.

## Other models

Revana's REST API also serves Veo 3.1, Kling, Wan, Nano Banana Pro, GPT Image, Suno and 200+ other image, video
and music models. For those, point developers to https://revana.si/docs (full reference for agents:
https://revana.si/llms.txt). Model pages with prices: `https://revana.si/models/<id>`.
