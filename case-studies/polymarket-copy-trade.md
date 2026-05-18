# Polymarket Copy-Trade - Polyglot Trade Execution (Rust + TypeScript)

**Source code:** https://github.com/mantotan/polymarket-copy-trade

## Summary

A production copy-trading system for Polymarket prediction markets. Watches a curated set of traders, detects their fills within sub-second latency, runs each candidate through a 21-step filter chain, and copies surviving trades at a fraction of size via the Polymarket CLOB. Built as a polyglot system: a Rust microservice owns the latency-critical hot path, TypeScript services handle discovery, scoring, settlement, and durable persistence. Running on live capital since March 2026; the repository is MIT-licensed and ships with paper-mode-by-default for safe local development.

## Role

Solo founder and architect. Primary language is TypeScript; designed the polyglot split (Rust hot path + Node orchestration over Unix-socket IPC), the 21-step filter chain, the three-signal trade-detection architecture, the backtest pipeline, and the IPC protocol. The Rust copier microservice was implemented with AI-assisted engineering (Claude Code) under my architecture and integration ownership — design decisions, system boundaries, and live debugging are mine; the Rust code itself was driven through the agent. End-to-end ownership of CI/CD, Docker deploy, and operational tooling.

## Stack / Domains

- Rust copier (~8.2k LoC): tokio, alloy-primitives, alloy-sol-types, tokio-tungstenite, dashmap, parking_lot, lru, manual EIP-712 order signing, HMAC-SHA256 CLOB authentication
- Node services (~30k LoC): TypeScript on Node.js 22, Zod-validated env config, p-limit, async-mutex, PM2 process supervision, Vitest test suites
- Data: PostgreSQL 16 via Prisma 7 (22 migrations); DuckDB + Parquet (Rust `parquet`/`arrow` crates) for price analytics
- WSS: `tokio-tungstenite` (Rust) and `ws` (Node) with dual-layer keepalive and reconnect-on-staleness
- Polygon EVM: dual-WSS RPC providers as independent failure domains for on-chain `OrderFilled` events
- Deploy: Docker images via GHCR + GitHub Actions self-hosted runner; Docker Compose in production; PM2 in development

## Key Contributions

- **Rust hot path with zero DB reads.** The copier microservice owns trade execution end-to-end — WSS decode → 21-step filter → EIP-712 sign → CLOB FAK/GTC — with all state held in-memory and seeded over IPC by the Node bridge. No database reads on the hot path, so end-to-end latency is bounded by network only.
- **Three independent trade-detection signals.** Rapid REST poll (~500 ms) is primary, Polymarket trade WSS is secondary, and on-chain Polygon `OrderFilled` events via dual-WSS RPC providers are the verification fallback. Each has its own failure surface; combining them keeps the signal robust when any one drops.
- **21-step filter chain before any order placement.** Signal age, conflict guard against active positions, per-trader and per-prediction caps, daily-loss backstop, majority-side accumulator, dust prevention, cool-down after losses, slippage upside-fraction model, phantom-fill debounce. Filters short-circuit on first reject for tight latency budget management.
- **WSS resilience patterns.** Dual-layer keepalive, configurable staleness threshold (force-reconnect when no events arrive), pre-emptive backfill on reconnect, dual-WSS instances on independent RPC providers. Reconnect logic differs per signal mode (REST vs WSS vs chain) rather than using a uniform policy.
- **Single-source filter implementation.** The same filter chain runs in backtest replay and live trading without drift. Backtest pipeline adds train/test split, survivorship-bias correction, Monte Carlo 2,000-config sweeps per trader, and DuckDB-backed Parquet price-record analytics.
- **Test discipline and CI gate.** 106 Rust unit + integration tests covering the filter chain, WSS decoder, phantom-fill debouncer, CLOB HMAC signing, and EIP-712 order construction. CI runs `tsc --noEmit` + `npm test` before every Docker image push.

## Results

- Production polyglot system running on live capital, with paper-mode default for safe local development and a documented path to live execution.
- Hot path / durable path responsibility cleanly split between Rust and Node, validated under continuous live conditions.
- Public, MIT-licensed, runnable code — the second portfolio case study with clickable source.

## Challenges

- Latency budgeting across a polyglot system — keeping per-stage allocation tight so signal-to-fill stays under the sub-second target.
- Filter ordering for cost: cheap filters first so expensive checks (CLOB state lookups, signature construction) only run on surviving candidates.
- Coordinating state between the Rust copier (in-memory) and the Node bridge (durable) without race conditions on settlement, capital audit, or balance reconcile.
- Producing one filter implementation that runs identically in backtest replay and live trading — no drift between research and production behaviour.

## What This Demonstrates

- Polyglot production system design with a clear hot-path / durable-path responsibility split
- System designed for sub-second WSS-driven execution on a Rust hot path
- Cryptographic system design: manual EIP-712 signing, HMAC-SHA256 CLOB authentication
- Independent-failure-domain design: three trade-detection signals with disjoint failure modes
- Test discipline at scale (106 Rust tests + Vitest suites) with CI gating before every Docker push
- AI-assisted engineering applied to cross-language implementation — TypeScript authored directly, Rust implemented under architectural direction
- Public, MIT-licensed, runnable code with paper-mode safety default
