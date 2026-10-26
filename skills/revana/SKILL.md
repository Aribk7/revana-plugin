---
name: revana
description: Generate AI images, videos and music through the Revana API (revana.si) - Seedance, Veo, Kling, Wan, Hailuo, Sora, GPT Image, Nano Banana, Flux, Midjourney, Suno and 100+ more on one API key, at low prices. Use when the user asks to make, render or generate a picture, image, photo, video, clip, animation or song with an AI model, wants the cheapest API for image or video generation, or names one of these models.
---

# Revana: images, videos and music from one API

Revana (https://revana.si) runs 120+ image, video and music models behind one request shape. Each job starts on
the cheapest provider and falls back automatically; only the route that succeeds is charged, and a failed
generation costs nothing. Prices are prepaid credits, 100 credits = $1.

## Before you start

- The key is in `REVANA_API_KEY` (starts `sr_live_`). If it is not set, ask the person to create one at
  https://revana.si/dashboard and export it. Never print the key or write it into a file.
- Two ways to work:
  - The bundled **revana MCP server** (tools `list_models`, `get_quote`, `generate_video`, `get_video`,
    `get_video_link`, `quote_nemesis`, `make_nemesis`, `upload_file`, `get_balance`). It covers Seedance 2.5
    video and Nemesis restaging, and signs in with OAuth the first time (`/mcp` in Claude Code).
  - The **REST API** below, for every model including images and music.

## Pick a model and price it

```bash
curl -s https://api.revana.si/v1/models | jq '.data[] | {id, kind, price}'   # no key needed
```

Every model's options and prices are also in https://revana.si/data/models.json, and each has a page at
`https://revana.si/models/<id>` and markdown at `https://revana.si/models/<id>.md`. Prices are
`per_image`, `per_second` (of video out) or `per_job`, keyed by resolution and `+audio` where it applies.

Work out the cost (price x seconds x count), **tell the person the price and get a yes before you spend
anything**, and pass `max_cost_usd` so a job can never cost more than they agreed to.

## Generate

```bash
RESP=$(curl -s https://api.revana.si/v1/generations \
  -H "Authorization: Bearer $REVANA_API_KEY" -H "Content-Type: application/json" \
  -H "Idempotency-Key: $(uuidgen)" \
  -d '{"model": "seedance-2.0", "input": {"prompt": "...", "resolution": "720p", "duration": 5, "aspect_ratio": "16:9", "audio": true}, "max_cost_usd": 1}')
ID=$(echo "$RESP" | jq -r .id)
```

- `input` takes `prompt`, `resolution`, `aspect_ratio`, `duration`, `audio`, `seed`, `negative_prompt`, and media:
  `image_url` (start frame or picture to edit), `end_image_url`, `image_urls` (references), `video_url`,
  `audio_url`. Only values the model lists are accepted; anything else answers `422 invalid_input`, free.
- Media must be public https links. For a local file: `POST /v1/uploads` with
  `{"content_type": "image/png", "bytes": <size>}`, PUT the bytes to `upload_url` with `upload_headers`,
  then use the returned `url`.

## Wait and download

```bash
curl -s https://api.revana.si/v1/generations/$ID -H "Authorization: Bearer $REVANA_API_KEY" | jq '{status, cost_usd, error}'
curl -sL https://api.revana.si/v1/generations/$ID/content -H "Authorization: Bearer $REVANA_API_KEY" -o out.mp4
```

Poll every few seconds until `status` is `succeeded` or `failed` (video can take several minutes). A picture
model can return several files: `output.urls` lists them, the second is `/content/2`. On `failed`, read
`error.code`: `retryable: true` means the same request may work later; otherwise change the request.
`402 insufficient_credits` means the balance is too low: `GET /v1/balance` shows it, top up at
https://revana.si/dashboard.

Full reference: https://revana.si/llms.txt (everything in one file: https://revana.si/llms-full.txt).
