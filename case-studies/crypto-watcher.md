# Crypto Watcher - Realtime Market Data & Signals Platform

## Summary

A multi-service realtime crypto market-data and signals platform spanning data ingest, pattern detection, distributed backtesting, paper/live order execution, AI-assisted news ingest, and mobile + web clients. Built as a Hato Labs in-house product across nine repositories, reaching full production readiness before being archived.

## Role

Founder and lead engineer at Hato Labs. Primary language is TypeScript; designed the overall multi-service architecture and owned the TypeScript API + client surfaces end-to-end. The Python/Go quant and execution services were implemented with AI-assisted engineering under my architectural and integration ownership — design, service boundaries, data contracts, and operational debugging are mine. Occasional contractor help on a subset of services.

## Stack / Domains

- Python: Pandas, NumPy, Polars, ClickHouse, DuckDB, FastAPI, GraphQL (Strawberry), SQLAlchemy, asyncio, websockets
- Go: WebSocket data service, paper-trading position monitor (SL/TP triggers)
- TypeScript: NestJS backends (signal logger, order manager, bot API, news ingest), Next.js web + React Native/Expo mobile
- Data: Redis TimeSeries + pub/sub, Postgres with Prisma, ClickHouse for chart-data service, Parquet for historical OHLCV
- Exchange / Domain: Binance Futures via CCXT, leverage-aware backtesting (up to 100x), risk-based position sizing
- AI / Automation: Google Generative AI for news content processing; self-hosted n8n workflows for news scraping and ingest orchestration
- Ops: Docker Compose across staging + production, multi-environment configuration

## Key Contributions

- Designed the cross-service architecture: realtime collectors → signal detector → order/paper managers → API → clients.
- Built the realtime data pipeline combining historical Parquet/ClickHouse storage with live Redis TimeSeries fed by a Go WebSocket service.
- Implemented pattern detection (double bottom/top with ATR-based confirmation) with TradingView integration for visualization.
- Built a futures backtesting engine with leverage, risk-based sizing, distributed worker pool, and comprehensive metrics.
- Built and wired the trade subsystem: Signal Logger, Order Manager (CCXT/Binance), Paper Order Manager, and the Go-based Paper Trading Monitor for SL/TP automation.
- Built the news ingest pipeline: self-hosted n8n workflows scrape and pre-process news sources, feeding a NestJS service that uses Google Generative AI to extract downstream content signals.
- Built the consumer surfaces: web app, mobile app (Expo), and operator bot frontend with JWT auth, OAuth (Google), rate limiting, and OpenAPI docs via Scalar UI.

## Results

- Nine-repository platform reached production readiness across staging and production environments under solo technical leadership.
- Distributed backtesting across multiple workers with consistent reproducibility from the same Parquet/ClickHouse sources.
- Realtime end-to-end latency budget held by separating Go WebSocket ingestion from Python signal computation and TypeScript order execution.
- Released as a Hato Labs experiment; the platform reached full operational readiness, and was archived after the early product-market-fit signal did not justify continued solo investment. The engineering artifacts remain a complete reference for the realtime-data, multi-service backend, and exchange-integration work it represents.

## Challenges

- Maintaining data integrity and time alignment between historical Parquet, live WebSocket, and ClickHouse for the chart-data service.
- Coordinating a polyglot system: Python for signal/quant work, Go for hot-path throughput, TypeScript for API + clients.
- Designing for both paper and live trading modes so the same signal path could be validated safely before production capital was at risk.

## What This Demonstrates

- Multi-service backend architecture under one technical lead
- Cross-language system design across Python, Go, and TypeScript with TypeScript directly authored and Python/Go implemented under architectural direction
- Realtime data systems with Redis TimeSeries, ClickHouse, WebSockets
- Workflow automation with self-hosted n8n for data ingest orchestration
- Domain knowledge of exchange APIs, futures markets, and risk management
- Solo product delivery at platform scope, with production observability discipline
