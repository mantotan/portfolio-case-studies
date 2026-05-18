# Octonpus - Solana Web3 Game

## Summary

A Solana-based Web3 game built at Hato Labs as an in-house product. Combines a Cocos2D game client (TypeScript with the TON Cocos SDK), a Go backend API, a TypeScript purchase-flow service, and an Anchor (Rust) Solana program for in-game purchases.

## Role

Founder of Hato Labs and technical lead on the project. Led architecture and delivery across game client, backend, purchase flow, and on-chain program, with a contractor team handling specialist implementation (game client, Go backend, Solana program).

## Stack / Domains

- Game client: TypeScript, Cocos2D, `@ton/cocos-sdk`
- Game backend: Go (chi router, pgx + Postgres, validator, logrus)
- Purchase backend: TypeScript + Express, Prisma, tweetnacl (signature verification)
- Smart contract: Solana, Anchor framework, Rust + TypeScript test harness
- Purchase frontend: Next.js, `@solana/web3.js`
- Landing: Next.js

## Key Contributions

- Designed the five-repo architecture spanning game client, two backends, smart contract, and landing surfaces.
- Defined the purchase flow: client signs a Solana transaction → purchase backend verifies via tweetnacl → on-chain Anchor program records the buy.
- Set the technology choices (Cocos for the game runtime, Anchor for the on-chain program, Go for the game API) and coordinated delivery across a multi-contractor team.

## Results

- Functional Solana Web3 game with on-chain purchase flow shipped to an early audience.
- Multi-language stack (TypeScript game, Go backend, Rust on-chain) delivered under a single technical lead.
- Released as a Hato Labs in-house bootstrap; the game did not reach sustained traction and was wound down. The engineering artifacts remain a complete reference for the Web3 game architecture and Solana purchase flow it represents.

## Challenges

- Bridging a Cocos game runtime with a Solana wallet signing flow inside the game UX.
- Coordinating three runtimes (TypeScript game, Go backend, Rust on-chain) and keeping their contracts aligned.

## What This Demonstrates

- Web3 game architecture with on-chain purchase logic
- Solana / Anchor smart-contract delivery
- Polyglot stack leadership (TypeScript + Go + Rust)
- Technical-lead role across a mixed-skill contractor team
