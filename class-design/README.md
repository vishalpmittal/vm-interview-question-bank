# Class Design Questions

Question bank for object-oriented / class design interviews (sometimes called OOD or LLD — low-level design). Focus is on identifying entities, relationships, and behaviors; choosing the right abstractions; and applying SOLID principles and design patterns appropriately.

## Suggested Topics

- Real-world systems (parking lot, elevator, vending machine, ATM, hotel reservation)
- Games (chess, tic-tac-toe, blackjack, poker, deck of cards)
- Domain modeling (library management, movie ticket booking, food delivery, ride hailing)
- Data structures with constraints (thread-safe LRU cache, rate limiter, event bus)
- Concurrency-aware designs (producer/consumer, connection pool, scheduler)

## What Interviewers Look For

- **Clarifying scope** — surfacing constraints, actors, and use cases before designing
- **Entity identification** — picking the right classes, not too many, not too few
- **Relationships** — composition vs. inheritance, has-a vs. is-a, multiplicity
- **Behavior placement** — methods on the class that owns the data
- **SOLID application** — especially Open/Closed and Single Responsibility
- **Design patterns** — used when they fit, not forced (Strategy, Factory, Observer, State)
- **Extensibility** — how the design accommodates likely future changes
- **Concurrency** — threading, locking, and invariants where relevant

## File Conventions

- One markdown file per design problem (e.g., `parking-lot.md`, `elevator-system.md`).
- Each file should include:
  - **Prompt** — the question as posed
  - **Clarifying questions** — scope, actors, constraints
  - **Entities & relationships** — classes, attributes, key associations (consider a simple class diagram in text/Mermaid)
  - **Core behaviors / interfaces** — methods and their contracts
  - **Design patterns applied** — and why
  - **Extension points** — how to add new behavior without rewriting existing code
  - **Trade-offs** — what was simplified and why
