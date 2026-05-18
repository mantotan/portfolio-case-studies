# Snuggle Rhino - NFT Mint Platform

## Summary

NFT mint platform delivered for a Hato Labs client. Built around a two-contract design (separate mint logic + NFT collection contract) with allowlist support via OpenZeppelin Merkle Tree, a NestJS mint orchestration backend, and a Next.js mint experience. Hato Labs led the engagement end-to-end, from NFT art direction through smart-contract delivery and the mint event.

## Role

Founder of Hato Labs and end-to-end lead on the engagement. Set the two-contract pattern, defined the mint orchestration backend, oversaw the NFT art produced by the in-house designer, and coordinated the engineering contractor team across smart contracts, backend, and frontends.

## Stack / Domains

- Smart contracts: Solidity, Foundry (Forge + Cast + Anvil), OpenZeppelin Merkle Tree for allowlist proofs
- Backend: NestJS, Prisma, ethers.js + viem (dual-client), AWS SDK, OpenZeppelin merkle-tree library, JWT, Swagger
- Mint frontend: Next.js, viem, wagmi (wallet connection)
- Main frontend: Next.js, Jotai, Lottie, Motion, tailwindcss-animate
- Coming-soon site: Next.js
- Design: NFT artwork delivered by the Hato Labs in-house designer under client art direction

## Key Contributions

- Owned the engagement end-to-end as Hato Labs lead: scope, architecture, art, smart-contract design, backend, frontends, and mint event coordination.
- Designed the two-contract architecture, separating mint logic (mintability, pricing, phases) from the NFT collection contract itself for cleaner upgrade and audit surfaces.
- Selected and integrated OpenZeppelin Merkle Tree for the allowlist mint pattern, both in the contract and in the backend that generates and serves proofs.
- Specified the mint orchestration backend (NestJS + Prisma), including the off-chain pieces: allowlist management, proof generation, mint phase configuration, and metadata service touchpoints.
- Directed delivery across the contractor team — smart contracts and backend (one engineer), mint and main frontends (another engineer) — while keeping consistent contract interfaces.

## Results

- Mint executed successfully with no security incidents or contract issues over the live mint period.
- Two-contract pattern (mint logic + collection) delivered as planned, with allowlist proofs served reliably from the backend.
- Engagement concluded after the live mint, per the client's roadmap; the on-chain artifacts remain on Ethereum.

## Challenges

- Balancing allowlist UX (smooth wallet connection, clear eligibility messaging) against gas-cost and contract-complexity tradeoffs.
- Coordinating a contractor team across Solidity, TypeScript backend, and Next.js frontend with consistent contract interfaces and a tight mint-day window.
- Operating across both art delivery and engineering delivery on a fixed client timeline.

## What This Demonstrates

- End-to-end ownership of a Web3 client engagement, from art direction through on-chain delivery
- Production Solidity workflow with Foundry tooling and a no-incident live mint
- Web3 architecture patterns: multi-contract design, allowlist via Merkle proofs, dual EVM clients (ethers + viem)
- Coordination of a mixed-skill contractor team on a Web3 product under a client deadline
- Clean separation between on-chain logic and off-chain orchestration
