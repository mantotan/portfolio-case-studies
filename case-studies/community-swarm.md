# Community Swarm - Web3 Community Management SaaS

- Website: https://communityswarm.com
- Status: Feature-complete, prod-deployed (backend API healthy with multi-week uptime), commercial launch paused

## Summary

Community Swarm is a SaaS platform that helps Web3 projects manage their Telegram and Discord communities. It pairs an AI-powered analytics dashboard — so project teams can understand what their community is saying — with AI community moderators deployed as project-operated accounts on Telegram and Discord under each customer's authorization. Each moderator is wired to a project-specific knowledge base and an LLM API to deliver on-brand, context-aware answers in real conversations.

The platform was built solo end-to-end across five repositories using a polyglot stack (TypeScript backend, Python AI/bot services, Next.js frontends), with AI-assisted engineering throughout. It is feature-complete and operationally ready; commercial launch is paused while focus is on other work.

## Role

Solo founder, architect, and builder. Designed and shipped every service in the platform — backend API, Python bot/AI services, frontend dashboard, marketing landing, and documentation site — with AI-assisted engineering throughout the build.

## Stack / Domains

- Backend (TypeScript): NestJS, Prisma, Postgres, Redis (ioredis), JWT, OTP (otplib), Socket.io (WebSockets), ethers.js (Web3 auth)
- Services (Python): SQLAlchemy + asyncpg, Redis, Pydantic, python-telegram-bot, Telethon (for project-account integrations), discord.py, Google Generative AI, APScheduler, structlog
- Frontend (TypeScript): Next.js, Radix UI, TanStack Query, Recharts, Playwright e2e + Vitest unit tests
- Landing: Next.js, React Three Fiber + Three.js
- Docs: Nextra
- Observability: Sentry across all services
- Ops: Docker Compose (dev + prod), proxy-aware HTTP for resilient bot connectivity

## Key Contributions

- Designed and shipped the product end-to-end: chat analytics dashboard, AI community moderators, per-project knowledge bases, and LLM-backed response generation, all under a single solo build.
- Designed a five-service polyglot architecture: TypeScript API for transactional concerns (auth, billing, projects, knowledge bases), Python service layer for AI processing and chat-platform integration, Next.js frontend, and a dedicated docs site.
- Built project-account Telegram + Discord moderators (not webhook bots) so they participate as first-class community members, with shared scheduling and per-project knowledge-base wiring in Python.
- Implemented per-project knowledge bases that condition LLM responses, so each moderator speaks in its project's voice and within its project's facts.
- Implemented wallet-based authentication using ethers.js alongside conventional JWT + OTP flows.
- Wired Sentry across all services for production observability and cross-runtime error correlation; established a test pyramid (Playwright e2e, Vitest unit, NestJS testing).
- Delivered the entire platform using AI-assisted engineering as a deliberate operating model.

## Results

- Feature-complete, operationally ready SaaS platform built solo across five repositories.
- Polyglot architecture validated in development: Python services for AI and chat-platform integration, TypeScript backend for API and Web3 concerns, with a clean service boundary between them.
- Demonstrates founding-engineer delivery velocity: a product with chat-platform integrations, AI conversational layer, analytics dashboard, knowledge-base management, and authentication flows, shipped end-to-end by one engineer.

## Challenges

- Operating project-account chat-platform integrations (vs webhook bots) reliably under rate limits, platform-side automation defenses, and proxy/network constraints.
- Cross-runtime coordination: defining stable contracts between Node and Python services without over-engineering.
- Keeping per-project knowledge bases, conversation memory, and LLM prompt scaffolding consistent across bots and platforms.

## What This Demonstrates

- Founding-engineer velocity: a multi-service SaaS shipped to operational readiness by one engineer
- Polyglot microservices architecture (TypeScript + Python under one platform)
- Production AI-product engineering: knowledge-base management, LLM-backed conversational agents, analytics on chat data
- Bot / automation platform design for Telegram and Discord, including project-account integration
- Web3 authentication patterns alongside conventional auth
- AI-assisted engineering as a delivery methodology, applied across an entire product
