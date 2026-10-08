# WHEN Markets — Solana Prediction Market Case Study

> **Sanitized portfolio case study of proprietary engineering work.** The original application and production source code remain private.

WHEN Markets was a full-stack prediction-market platform built around Solana. The product combined a Next.js web application, Flutter mobile client, NestJS backend services, PostgreSQL application data, wallet/transaction flows, and on-chain program interactions.

The interesting engineering problem was coordinating **user experience, backend state, blockchain state, and transaction lifecycle** without allowing those boundaries to become ambiguous.

## At a Glance

| Area | Details |
|---|---|
| Product | Solana prediction-market platform |
| Web | Next.js, React, TypeScript |
| Mobile | Flutter |
| Backend | NestJS, Node.js |
| Data | PostgreSQL, Prisma |
| Blockchain | Solana, Anchor |
| Scope | Full-stack product and blockchain integration |
| Source | Private / proprietary |

## My Engineering Scope

- Next.js web application development
- Flutter mobile application development
- NestJS API/backend development
- PostgreSQL-backed application services
- Solana wallet and transaction flows
- Prediction-market program integration
- Market lifecycle and settlement workflows
- Debugging, performance, architecture, and release packaging

## System Architecture

```text
             ┌──────────────────────┐
             │   Web / Mobile Apps  │
             │ Next.js / Flutter    │
             └──────────┬───────────┘
                        │
             ┌──────────▼───────────┐
             │    NestJS APIs       │
             │ Application Services │
             └───────┬───────┬──────┘
                     │       │
              ┌──────▼───┐ ┌─▼─────────────┐
              │PostgreSQL│ │    Solana     │
              │App State │ │Programs / TXs │
              └──────────┘ └───────────────┘
```

The diagram is intentionally high-level. Proprietary service boundaries, credentials, addresses, and production infrastructure are omitted.

## Core Engineering Challenges

### 1. Coordinating off-chain and on-chain state

The platform combined application state with blockchain state. A key design concern was making ownership of state explicit:

- UI state represents what the client currently knows.
- Backend state supports application workflows and APIs.
- Solana state represents authoritative on-chain program state where applicable.
- Transaction state represents an asynchronous lifecycle rather than a single success/failure event.
- Market lifecycle state determines what actions are valid at each stage.

### 2. Wallet and transaction lifecycle

Wallet interactions were part of user-facing trading/staking flows. The engineering model therefore treated a transaction as a lifecycle: `requested → submitted → confirmed`, with failure and retry paths where appropriate.

A wallet action being initiated is not equivalent to the corresponding blockchain state transition being confirmed.

### 3. Market lifecycle

Prediction markets require more than a trading interface. The platform needed to account for market creation, participation, state transitions, and settlement.

The architecture therefore treated market lifecycle as a first-class concern instead of distributing settlement assumptions throughout individual screens.

### 4. Web and mobile consistency

The product had both web and Flutter clients. Shared backend contracts allowed both clients to consume common application services while keeping platform-specific presentation concerns inside each client.

## Technology Map

- **Frontend:** Next.js, React, TypeScript
- **Mobile:** Flutter
- **Backend:** NestJS, Node.js
- **Database:** PostgreSQL
- **ORM:** Prisma
- **Blockchain:** Solana
- **On-chain development:** Anchor / Solana programs
- **Async flows:** backend services + blockchain transaction state

## Engineering Decisions

### Explicit state boundaries
The system distinguishes application state, blockchain state, and transaction state rather than treating them as interchangeable.

### Thin clients
Complex business rules should not be independently reimplemented in web and mobile clients. Shared backend contracts provide a common application boundary.

### Transaction-aware UX
Blockchain operations are asynchronous. UI flows therefore need to communicate intermediate states instead of assuming immediate finality.

### Lifecycle-driven design
Market behavior is easier to reason about when valid transitions are explicit. This also gives backend and client code clearer boundaries.

## What I Learned

**Authoritative state must be explicit.** When a product spans a database and a blockchain, unclear state ownership creates subtle inconsistencies.

**Transactions are state machines.** Submission, confirmation, failure, and retry are distinct states with different UX and operational implications.

**Cross-platform products benefit from shared service boundaries.** Web and mobile can evolve independently while consuming the same backend contracts.

## Public / Private Boundary

This repository contains only sanitized engineering documentation.

It intentionally excludes:
- Proprietary source code
- Private keys and signing material
- Production credentials
- Private user data
- Internal infrastructure details
- Client-confidential implementation details

The purpose of this repository is to demonstrate **engineering reasoning and system architecture**, not to reproduce the proprietary application.