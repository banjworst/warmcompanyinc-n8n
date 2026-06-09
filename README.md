# WarmCompanyInc — AI Agent Ecosystem

An autonomous, zero-upfront-cost AI agent ecosystem that generates revenue across multiple platforms. Agents run on a schedule, research trends, create content, and publish listings without manual intervention.

---

## Overview

WarmCompanyInc is a collection of AI-powered automation workflows built on n8n that operate five independent revenue streams. Each stream is powered by a dedicated agent pipeline that handles everything from trend research to publishing.

The core philosophy: **build once, earn continuously.** Revenue from early streams funds expansion into the next.

---

## Revenue Streams

| Stream | Platform | Status |
|--------|----------|--------|
| Print-on-Demand Store | Printify + Etsy | 🟡 In Progress |
| UGC Content | TikTok + Instagram | 🔲 Planned |
| Clip Engine | Streams + Podcasts | 🔲 Planned |
| YouTube Shorts | YouTube (Trending) | 🔲 Planned |
| Children's Content | YouTube Kids | 🔲 Planned |

---

## Stream 1 — Printify + Etsy Agent

### What it does

Runs every morning at 9am and autonomously:

1. Researches trending gift keywords via Google Trends (SerpApi)
2. Identifies the highest-interest keyword over the last 4 weeks
3. Generates a product design image using OpenAI's image generation API
4. Uploads the design to Printify
5. Creates a mug product with correct blueprint, variant, and pricing
6. Publishes the listing directly to the WarmCompanyInc Etsy shop

### Agent Pipeline

```
Schedule Trigger (9am daily)
        ↓
Fetch Google Trends (SerpApi)
        ↓
Extract Top Keyword (Code Node)
        ↓
Generate Product Image (OpenAI gpt-image-1)
        ↓
Upload Image to Printify
        ↓
Build Product Payload (Code Node)
        ↓
Create Printify Product
        ↓
Publish to Etsy
```

### Tech Stack

| Tool | Purpose | Cost |
|------|---------|------|
| n8n Cloud | Workflow automation backbone | Free trial / ~$20/mo |
| SerpApi | Google Trends keyword research | Free (100 searches/mo) |
| OpenAI API | Product image generation (gpt-image-1) | ~$0.04/image |
| Printify | Print fulfillment + mockup generation | Free |
| Etsy | Storefront + marketplace | $0.20/listing + % on sale |

### Economics

- **Sale price:** $22–30 per mug
- **Printify base cost:** ~$9–13
- **Etsy fees:** $0.20 listing + 6.5% transaction + ~3% payment
- **Margin per sale:** ~$5–10
- **Break-even:** First sale covers all listing costs

---

## Architecture

```
┌─────────────────────────────────────────────┐
│              n8n Cloud Instance              │
│         warmcompanyinc.app.n8n.cloud         │
│                                             │
│  ┌──────────┐   ┌──────────┐   ┌─────────┐ │
│  │ Stream 1 │   │ Stream 2 │   │   ...   │ │
│  │  Etsy    │   │  Social  │   │         │ │
│  └──────────┘   └──────────┘   └─────────┘ │
└─────────────────────────────────────────────┘
         │               │
    ┌────┴────┐     ┌────┴────┐
    │Printify │     │TikTok / │
    │  + Etsy │     │Instagram│
    └─────────┘     └─────────┘
```

Agents are built visually in n8n and connected to external APIs via HTTP Request nodes. Claude Code is used to build and modify workflows via the n8n MCP server.

---

## Setup

### Prerequisites

- n8n Cloud account
- Etsy seller account + developer app (Etsy API v3)
- Printify account connected to Etsy
- SerpApi account (free tier)
- OpenAI API account with credits

### Claude Code + n8n MCP Integration

Workflows are built and managed via Claude Code connected directly to the n8n instance:

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Authenticate
claude --dangerously-skip-permissions

# Add n8n MCP server
claude mcp add n8n-warmcompany https://warmcompanyinc.app.n8n.cloud/mcp-server/http --header "Authorization: Bearer YOUR_TOKEN"

# Start a session
claude
```

### Environment Variables / Credentials Needed

| Variable | Where to Get It |
|----------|----------------|
| `SERPAPI_KEY` | serpapi.com → Dashboard |
| `OPENAI_API_KEY` | platform.openai.com → API Keys |
| `PRINTIFY_TOKEN` | printify.com → Settings → API |
| `ETSY_KEYSTRING` | developers.etsy.com → Your App |
| `ETSY_SHARED_SECRET` | developers.etsy.com → Your App |
| `N8N_MCP_TOKEN` | n8n → Settings → Instance-level MCP |

> Never commit API keys to this repository. Store them in n8n's credential manager or as environment variables.

---

## Roadmap

**Phase 1 — Foundation** (Complete)
- Etsy shop live (WarmCompanyInc)
- Printify connected
- n8n Cloud instance running
- Claude Code connected via MCP
- Stream 1 workflow built and tested

**Phase 2 — First Revenue**
- Stream 1 running daily
- First 50+ listings published
- First sale generated
- Workflow export backed up to GitHub

**Phase 3 — Reinvest**
- Use Stream 1 revenue to fund Stream 2 (UGC + Social)
- Add text personalization to top-selling products
- Upgrade to Printify Premium once $200+/month

**Phase 4 — Full Ecosystem**
- All 5 streams live
- Agents hand off to each other
- Monthly revenue target: $1,000+

---

## Notes

- DALL-E 3 was deprecated by OpenAI on May 12, 2026. This project uses `gpt-image-1` instead.
- n8n free trial lasts 14 days. Export all workflows as JSON before trial ends as a backup.
- Etsy API rate limits new shops — ramp up slowly (5–10 listings/day for first 2 weeks).
- YouTube Kids content is subject to COPPA regulations. No targeted ads on made-for-kids videos.

---

## License

MIT
