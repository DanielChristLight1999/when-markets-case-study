# Engineering Decisions

## Decision 1 — Make state ownership explicit

### Problem
The product combines PostgreSQL-backed application data with Solana program state. Both can contain information about the same business object without having the same authority.
### Approach
Define which layer owns each piece of state and treat derived values as derived values.
### Benefit
This makes synchronization problems easier to diagnose and prevents a stale application value from being treated as blockchain truth.

## Decision 2 — Treat blockchain actions as asynchronous

### Problem
A wallet signature or transaction submission does not necessarily mean the intended state transition has completed.
### Approach
Model transaction progress explicitly: `requested → submitted → confirmed`, with failure and retry paths.
### Benefit
The UI can communicate the actual state of the operation instead of providing premature success feedback.

## Decision 3 — Keep clients thin

### Problem
When web and mobile clients each implement business rules independently, their behavior can drift.
### Approach
Use shared backend contracts for common application logic while keeping platform-specific UI concerns in each client.
### Benefit
The same core behavior can be consumed by multiple clients without duplicating complex rules.

## Decision 4 — Treat market lifecycle as a first-class model

### Problem
Prediction markets have state transitions beyond simple CRUD operations.
### Approach
Think in terms of lifecycle states and valid transitions rather than scattering assumptions across UI components.
### Benefit
Settlement and participation behavior becomes easier to reason about, test, and evolve.

## Decision 5 — Keep the public case study sanitized

The original system was proprietary. The public repository therefore documents architecture and engineering reasoning without publishing implementation details that could expose private infrastructure or confidential information.