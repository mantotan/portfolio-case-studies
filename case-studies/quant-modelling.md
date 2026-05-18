# Quant-Modelling - ML Prediction Markets + Autonomous Research Loop

**Source code:** https://github.com/mantotan/quant-modelling

## Summary

A quant ML pipeline trading crypto-binary prediction markets on Polymarket. Twin LightGBM models — **Sentinel** (predicts the next bar from completed bars) and **Pulse** (predicts the current bar from a t=0.80 intra-bar snapshot) — produce calibrated probabilities for 12 independent markets in parallel (BTC / ETH / SOL / XRP × 5m / 15m / 1h). Edge against live Polymarket odds is sized via fractional Kelly with per-asset correlation limits and a daily-loss kill switch. Wrapped around the model layer is a **multi-agent autonomous research loop** (13 agent definitions, 8 sentinel + 5 dutch) that designs experiments, runs HPO, audits results, and updates configs without supervision — driving 150+ research iterations to structural-floor exhaustion across all 12 models. Public, MIT-licensed.

## Role

Solo founder and architect. Primary language is Python; designed the twin-model architecture, the autonomous-research multi-agent loop, the validation pipeline (walk-forward + CPCV + PBO + isotonic time-aware calibration), the multi-timeframe risk overlay, and the planned Rust `qm-fast/` crate for the latency-critical inference path. AI-assisted engineering applied throughout — design decisions and validation discipline are mine; routine implementation work was driven through agentic workflows.

## Stack / Domains

- ML: LightGBM, Optuna HPO, treelite-compiled inference, isotonic + time-aware bucket calibration, scikit-learn
- Data: Polars, DuckDB, Hive-partitioned Parquet, TimescaleDB
- Validation: walk-forward with purge + embargo, CPCV with PBO (probability of backtest overfit), Deflated Sharpe
- Risk: fractional Kelly (0.25x), correlation limits across timeframes per asset, daily-loss circuit breaker
- Polymarket: py-clob-client, web3 + eth-account (manual EIP-712 order signing)
- Config: Hydra + OmegaConf
- Observability: structlog with secret-redaction, Prometheus
- Quality: ruff (7 lint categories), mypy strict, pytest with unit / integration / benchmark splits
- Planned Rust fast path: `crates/qm-fast/` for sub-millisecond inference once execution latency becomes the binding constraint

## Key Contributions

- **Twin model design (Sentinel + Pulse).** Sentinel predicts the next bar from completed-bar features; Pulse predicts the current bar from a t=0.80 intra-bar snapshot. They fuse at the signal layer because each Polymarket contract (5m / 15m / 1h) resolves at the next bar close — both models matter at different points within a single window.
- **Honest validation discipline.** Walk-forward with purge + embargo, CPCV with PBO < 0.40, Deflated Sharpe > 0, OOS Brier < 0.25, OOS Expected Calibration Error < 0.05 as the keep gate. Acceptance criteria enforced before any model goes live.
- **Multi-agent autonomous research loop.** 13 agent definitions — 8 sentinel (`dispatch`, `researcher`, `strategist`, `auditor`, `analyst`, `builder`, `reconciler`, `reconciliation-dispatch`) + 5 dutch agents tuning the Dutch accumulation strategy in parallel. State lives in flat files under `autoresearch/` (`knobs.json`, `results.tsv`, `strategy.md`, `audit.md`, `phase.json`) — no message bus, no shared process, deliberately crash-resistant and auditable. 150+ iterations until structural floors confirmed across all 12 models.
- **treelite-compiled inference.** LightGBM models compiled to C for sub-millisecond live scoring. Polyglot Python + planned Rust `qm-fast/` crate for the hot path once latency becomes the constraint.
- **Multi-timeframe decision.** Each Polymarket contract is a separate market with separate odds; merging signals across timeframes would merge independent bets. Deploy 12 models independently instead, with a correlation-aware risk overlay capping same-asset exposure.
- **Live multi-asset trading deployed.** ETH + SOL + XRP at 5m intervals, $2/order, daily-loss kill switch, paper-vs-live reconciliation cycle that opens fixes when divergence is real (not assumed).

## Results

- 831 commits of preserved iterative history; the autonomous-research loop ran 150+ iterations to structural-floor exhaustion across all 12 models.
- 67 test files / 227 + tests passing; strict mypy + ruff in CI; CI gate runs `pytest tests/unit` before every Docker image push to GHCR.
- Public, MIT-licensed, paper-mode-default. Production live trading on ETH + SOL + XRP at 5m intervals.

## Challenges

- Backtest-to-live divergence investigation — keeping the same filter chain identical across replay and live, then running periodic reconciliation cycles that auto-open fixes when divergence is real (not assumed).
- Correlation-aware sizing across 12 simultaneous models — same-asset exposure is the failure mode; per-timeframe Kelly without an overlay would over-concentrate.
- Designing the multi-agent loop to be crash-resistant — flat-file state instead of a shared Python process so any agent can crash and be restarted without corrupting the others' work.

## What This Demonstrates

- ML system design at depth: twin complementary models + treelite-compiled inference + isotonic time-aware calibration + walk-forward CPCV with PBO as a gate
- Multi-agent autonomous-research orchestration with deliberately simple flat-file state
- Validation rigor before deployment, not after divergence
- Polyglot performance engineering — Python orchestration, Rust hot path planned and architected
- Honest research note-keeping (reconciliation cycles that document real backtest-to-live gaps rather than hiding them)
- AI-assisted engineering applied to a substantial, multi-component system with clear architectural ownership
- Public, MIT-licensed, runnable code with paper-mode-default safety
