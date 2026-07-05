# Networking infrastructure PRD

## Summary

Toys connect to each other through one shared mechanism, not bespoke per-toy integrations: a small port contract that any toy can expose, and a dedicated connector toy that discovers ports, lets a user snap two together, and shows the resulting data flow live. Toys plug into each other like Lego bricks: same stud on every brick, regardless of what the brick builds. Because this component's source ships publicly alongside the toy, its code is a second curriculum, not just an implementation detail.

## Problem

Without a shared mechanism, every toy that wants to talk to another would invent its own integration, producing inconsistent UX and hidden, unreviewable complexity, exactly what CONSTRAINTS.md §4 (state legibility) warns against. Two connectivity needs identified earlier (a single connection between two specific toys, and many instances hubbed together for Kit 01's "botnets" content) turn out to be the same mechanism at different scale, so one infrastructure component covers both instead of two separate systems.

## Solution

### Port contract (shared code)

The one piece of genuinely shared code. A toy exposes zero or more named ports: a name, a direction (emit, accept, or both), and a small bounded data shape (a single value or short record, not an arbitrary blob). Kept intentionally minimal so it can't accumulate hidden complexity in shared code, the same reasoning CONSTRAINTS.md §1.2 applies to domain closure.

### Connector toy (its own Kit, own PRD later)

A toy like any other, scoped against the same CONSTRAINTS.md clauses:

- **Domain (§1):** discover local ports, connect two together, show data flowing across the connection. Nothing else.
- **Bounded state (§2):** a hard ceiling on simultaneous connections (e.g. 8 nodes), enforced at the GDScript level.
- **Zero-latency (§3):** connecting two ports shows data flow immediately.
- **State legibility (§4):** the connection graph and the packets on it are the entire UI. Nothing to inspect that isn't already on screen.
- **Failure reversibility (§6):** disconnect-all is one action, reachable from any state.

This same toy, configured with many nodes hubbed through one connector, is Kit 01's "agents as computing / botnets" demonstration. Hub-and-spoke through one visible toy replaces what would otherwise be an invisible many-to-many mesh, and gives §4 something concrete to point at.

### Scope for v1: local and LAN only

Connections use a manually entered local address. No signaling server, no relay, nothing to host indefinitely. This covers goal 4 (one toy driving another, two arenas facing off) and Kit 01's hive demonstration without taking on an always-on infrastructure dependency. Open-internet reach (which would need WebRTC signaling) is a later extension, not a v1 commitment.

### Code as a teaching artifact

This project's source is released publicly (see `docs/ARCHITECTURE.md` § Source availability), and for this component specifically, the code is part of the curriculum, not just the mechanism behind it. Practical implications:

- Prefer a simple, hand-rollable wire format over an opaque library call when the difference is pedagogically meaningful. A learner reading the connector toy's source should be able to see what a handshake and a packet actually look like, not just a function call into a networking library.
- This component is an explicit exception to the project's default "don't over-comment" convention. Where a comment explains the protocol itself rather than what the code obviously does, write it. Keep the exception scoped to this component; it does not relax commenting standards elsewhere.

## Success criteria

- A learner reading only the port-contract and connector-toy source, no external docs, can explain what a handshake and a packet are in this system.
- Two toy instances on the same machine or LAN can connect and exchange one value round-trip, with the connection visible within one user action.
- Kit 01's hive/botnet lesson reuses the connector toy directly rather than needing separate simulation code.

## Scope

**In:** port contract shape, connector toy domain and CONSTRAINTS compliance, local/LAN-only v1 scope, code-as-teaching-tool implications for this component.

**Out:** Kit numbering and naming for the connector toy (deferred to `docs/IDEAS.md` and its own PRD), open-internet transport, exact wire-format bytes (Phase 3), the connector toy's teaching content itself (its own PRD).

## Open questions

- Does "code and toy are both learning tools" generalize to every toy, or is it specific to this component because its protocol is itself the lesson? Recorded project-wide in ARCHITECTURE.md; revisit once a second toy's spec is written.
- Kit number and title for the connector toy in `docs/IDEAS.md`.
- Whether local/LAN-only remains the right v1 scope once the connector toy's own PRD is drafted, or whether a real user need pulls in open-internet reach sooner.
