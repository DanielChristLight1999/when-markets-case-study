# Architecture Notes

## Context

WHEN Markets combined three major execution environments:
1. **Client applications** — web and mobile experiences.
2. **Application services** — APIs and persistence for product workflows.
3. **Solana programs and transactions** — blockchain state and state-changing operations.

## Logical Flow

```text
User
  │
  ▼
Web / Mobile Client
  │
  ├──────────────► Solana: signing / transactions
  │
  ▼
NestJS Application API
  ├──────────────► PostgreSQL
  └──────────────► Solana: reads / coordination
  │
  ▼
Normalized application state
```

## State Ownership

| State | Primary concern |
|---|---|
| UI state | Loading, pending, confirmation and presentation |
| Application state | Product workflows and persisted records |
| Blockchain state | Program-owned on-chain state |
| Transaction state | Submission, confirmation and failure |
| Market lifecycle | Valid transitions and settlement status |

The important principle is that **a value can be useful without being authoritative**.

## Transaction Lifecycle

```text
User intent
    │
    ▼
Client validation
    │
    ▼
Transaction construction
    │
    ▼
Wallet signing
    │
    ▼
Submission
    │
    ├── failure ──► retry / recover
    │
    ▼
Confirmation
    │
    ▼
Application refresh
```

## Client Boundary

```text
Web Client ─────┐
                ├──► Application API ───► Data / Blockchain
Mobile Client ──┘
```

This reduces duplicated business logic and gives the product a consistent behavior model across platforms.

## Why the Architecture Matters

The difficult part of a blockchain product is often not the individual technology. It is coordinating systems with different consistency and timing characteristics.

The architecture therefore emphasizes:
- explicit state ownership
- transaction lifecycle awareness
- reusable application contracts
- lifecycle-driven market behavior
- clear separation between presentation and business rules