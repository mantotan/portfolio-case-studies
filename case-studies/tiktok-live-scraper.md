# TikTok Live Scraper - WebSocket Resilience & Proxy Infrastructure

**Source code:** https://github.com/mantotan/tiktok-live-scraper

## Summary

A self-hosted scraper that answers one question — is a TikTok-live niche actually worth streaming in? It tracks 10–50 streamers per niche, records every gift, comment, viewer count, and follow into Postgres, converts diamond flows into IDR revenue estimates, and lets me see per-streamer and per-niche economics over weeks and months. Most of the engineering effort goes into the boring-but-hard part: keeping the WebSockets connected to TikTok for hours without quietly dying.

This is the first project flipped public from the portfolio. The repo README carries the full design narrative; this case study is the index entry.

## Role

Solo founder, architect, and builder. Designed the system, made the architectural calls (Euler Stream + Decodo hybrid proxy mode, treating the WebSocket as a state machine, paying the pre-check cost to cut bandwidth ~90%), and debugged the production failure modes. Built with AI-assisted engineering throughout — implementation accelerated by Claude Code, design + debugging owned by me.

## Stack / Domains

- Node.js + TypeScript, Hono REST API, Prisma + PostgreSQL, Pino structured logging
- pnpm + Turborepo monorepo, PM2 process supervision
- WebSocket state-machine connection management for TikTok-live protocol
- Hybrid connection modes: Euler Stream Cloud (signed handshake, capped at 10) + Decodo residential proxies (direct WebSocket from a real consumer IP)
- Time-series aggregation in Postgres (deferring TimescaleDB until 1TB+)

## Key Contributions

- **Zombie WebSocket recovery.** Sockets that look alive but stop emitting events were the most expensive failure mode. Built a per-worker `lastEventTime` sweep that force-reconnects after a 5-minute silence, with a 2-strike loop guard and a subtle correctness rule: strikes only clear when a *real event* arrives after the last reconnect timestamp, not just because the TCP handshake succeeded.
- **Hybrid proxy infrastructure.** Cloud connections die at the 8-hour lifetime; residential proxies sometimes hit `DEVICE_BLOCKED`. Per-mode backoff and retry — uniform logic would have been wrong for either mode.
- **Bandwidth-aware pre-checks.** Polling 30 streamers with full HTML+WebSocket handshake bleeds residential bandwidth. Lightweight `roomId`-scoped status queries cut pre-check bandwidth ~90%, with a capped backoff to avoid getting stuck on a stale room ID after a streamer starts a new session.
- **Honest data modeling.** `connection_gaps` is an explicit table — a streamer who held 5K peak viewers for 3 hours straight is materially different from one who held 5K twice with a 30-minute gap. `SessionEndReason` includes `zombie_killed` as a first-class outcome so the failure mode is queryable, not just logged.

## Results

- Running self-hosted, validating live-niche economics over weeks of continuous capture.
- Zombie-WebSocket detection landed across three commits, with the final commit eliminating an infinite-reconnect loop caught only because the strike-clearing rule was tightened.
- First repo flipped public from the portfolio — chosen because the engineering depth is in production failure modes, not just CRUD.

## Challenges

- Diagnosing "alive but silent" WebSocket failures where TCP, close frames, and `onClose` all behaved correctly but no events arrived.
- Balancing per-mode failure surfaces (cloud-lifetime kill vs proxy-IP block) without leaking abstractions.
- Keeping pre-check optimization correct under a stale-`roomId` edge case that, uncapped, would have lost entire live sessions on popular streamers.

## What This Demonstrates

- Production debugging of distributed-system failure modes where naive reconnect logic fails
- WebSocket state-machine design and per-mode retry/backoff infrastructure
- Bandwidth-aware optimization with measured (~90%) reduction
- AI-assisted engineering as a delivery methodology, with honest separation of what AI did (implementation typing) and what I did (design, decisions, debugging)
- Public, MIT-licensed, runnable code — the only case study in this portfolio with clickable source
