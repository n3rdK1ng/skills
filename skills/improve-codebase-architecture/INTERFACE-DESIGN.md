# Interface Design

The user picked a deepening candidate and wants to explore alternative interfaces for the module. Walk through this with them — don't propose a single design and call it done.

## Frame the choice

An interface is everything a caller must learn to use the module safely. Each candidate interface implies a different *cost-of-learning* and a different *leverage payoff*. The best interface is the one where the cost is lowest and the leverage is highest — but those usually trade off, and the user has to feel the trade to pick well.

## Generate alternatives

Propose 2–4 candidate interfaces, not one. Naming-it-once locks the design in. Show variations and let the user feel the difference.

For each candidate, write:

- **Surface** — the signatures and types a caller sees
- **Contract** — what the interface promises (invariants, error modes, ordering)
- **What it absorbs** — which logic moves inside the module vs. stays outside
- **What it asks of callers** — what they still have to know

## Stress-test each candidate

Run these tests against every alternative:

- **Deletion test** — if you imagine this module not existing, does complexity vanish or re-scatter? The candidate must pass.
- **Two-adapter test** — name a plausible second implementation. If you can't, the seam isn't real and the interface will bend to the one caller it has.
- **Test surface** — sketch the tests you'd write against this interface. Are they exercising *behaviour*, or restating the implementation? If they're restating the implementation, the interface is shallow.
- **Caller learning cost** — what does a new caller have to learn to use this safely? Count invariants, ordering rules, required configuration. Lower is better.
- **Failure modes** — when this module misbehaves, where does the error surface? At the seam (good) or deep in the caller (bad)?

## Avoid common shapes

- **Configuration-heavy interfaces** — every flag is a thing the caller has to learn. Push policy inside; expose mechanism. If the module has nine bool parameters, callers have to know nine truth tables.
- **Pass-throughs** — if the interface just rewraps the underlying call, the module is shallow by definition. Delete it or absorb more.
- **Leaky abstractions** — if callers have to know which adapter is behind the interface, the seam isn't really there. The interface should be expressible without naming any implementation.
- **Sprawling shapes** — a deep module shouldn't grow a huge interface to stay deep. If the surface is widening to absorb more, look for a sub-module inside that wants its own seam.

## Decide

Pick one. Record the rejected alternatives only if the rejection reason is load-bearing — otherwise the noise outweighs the signal. If the rejection encodes a constraint future explorers would otherwise re-litigate, lift it to an ADR (see [`../domain-model/ADR-FORMAT.md`](../domain-model/ADR-FORMAT.md)).
