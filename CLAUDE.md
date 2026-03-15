# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ChillClaw（躺赚龙虾）is an OpenClaw AI Agent skill for Binance, built for the Binance x OpenClaw AI Agent Hackathon (deadline: 2026-03-18). It automates three scenarios for Binance users: Alpha token research, Booster activity tracking, and Earn optimization.

**This is NOT a traditional codebase** — it's an OpenClaw skill definition (no build/test/lint steps). The project consists of:
- `skill/chillclaw/SKILL.md` — The skill specification (workflows, API details, prompt instructions)
- `skill/chillclaw/config.example.json` — Configuration template
- `ChillClaw-PRD.md` — Full product requirements document

## Architecture

Three-layer design: **Data Layer → Agent Layer → Output Layer**

### Data Sources (6 total)
| Source | Auth | Purpose |
|--------|------|---------|
| **Surf AI** (`api.asksurf.ai`) | Bearer token (`SURF_AI_API_KEY`) | Professional token research reports, scoring, AI chat |
| **Binance Skills** (7 skills via OpenClaw) | `BINANCE_API_KEY` + `SECRET` for Spot only | On-chain queries (no key needed), CEX account access (key needed) |
| **OpenNews MCP** | `OPENNEWS_TOKEN` | Binance activity/news discovery |
| **Booster Calendar** (`booster-unlock.vercel.app/api.json`) | None | Token unlock schedules (self-built) |
| **Alpha123** (`alpha123.uk/api/data`) | None (Cloudflare protected, use `fetch()`) | New Alpha token discovery |
| **Binance Public API** | None | Real-time price queries |

### Three Core Scenarios

**A. Alpha Research** — New token discovery → Surf AI report + contract audit + Smart Money signals → investment report
**B. Booster Tracking** — Activity discovery → portfolio matching → unlock notifications → sell/hold analysis
**C. Earn Optimization** — Idle asset scan → compare yields → recommend products → maturity reminders

### Key Design Principles
1. **LLM only handles decisions requiring judgment** — Cron scripts handle data fetching; Claude synthesizes multi-source data
2. **Every notification must have decision value** — Not "BTW unlocks tomorrow" but "BTW unlocks tomorrow, SM is selling, suggest exit"
3. **Deep skill integration** — Each scenario orchestrates 3-5 APIs, not single API calls

## Surf AI Model Selection

| Model | Latency | Use Case | Stream Required |
|-------|---------|----------|-----------------|
| `surf-ask` | 10-30s | Default for quick research, batch scanning | No |
| `surf-research` | ~5min | Only when user explicitly requests deep analysis | **Yes (mandatory)** |
| `surf-1.5` | 10-30s | Latest model, can replace surf-ask | No |

**Important**: `ai-report` and `ai-recommended-assets` endpoints return Korean by default. For Chinese content, use `chat/completions` with Chinese system prompt.

## Binance Skills — Key Distinction

- **On-chain data skills** (no API key): Query Address Info, Token Info, Token Audit, Market Rank, Trading Signal — all public blockchain data
- **CEX account skill** (requires API key): Binance Spot — exchange account balances, positions, orders
- `Query Address Info` queries **on-chain wallet addresses**, NOT Binance CEX accounts

## Development Workflow

1. **Phase 1** (Claude Code): Validate all data source connectivity
2. **Phase 2** (Claude Code): Implement core business logic for all three scenarios
3. **Phase 3** (OpenClaw): Deploy with Telegram channel + Cron automation
4. **Phase 4**: Contest submission on X/Binance Square

## Slash Commands (in OpenClaw)

| Command | Function |
|---------|----------|
| `/chillclaw` | Daily overview: upcoming unlocks + new activities + idle assets |
| `/chillclaw alpha` | Alpha token scan + auto research |
| `/chillclaw unlock` | 7-day unlock calendar + sell analysis |
| `/chillclaw booster` | Latest Booster/Launchpool activities |
| `/chillclaw earn` | Portfolio scan + Earn recommendations |
| `/chillclaw {SYMBOL}` | Single token deep research |

## Unlock Date Calculation

From Booster Calendar API: `unlockDate = tgeDate + daysFromTGE` (days). `pending: true` means tokens not yet distributed.
