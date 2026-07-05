# Toy Program Specification v1.0

This file represents the constraints of toy programs, for thinking purposes.

## 0. Scope

This document specifies the design invariants a system MUST, SHOULD, or MAY satisfy to qualify as a "toy program." It is written as an architectural contract, not a marketing description — every clause exists to rule something in or out. Reference implementations: Mattel Football (1977), 150-in-1 electronics kit, Big Trak, STOS, PICO-8.

Terminology follows RFC 2119: MUST/MUST NOT = hard requirement, SHOULD/SHOULD NOT = strong default with justified exceptions, MAY = optional.

---

## 1. Domain Closure (MUST)

**1.1** The system MUST define a fixed target domain (e.g., "2D sprite games," "sequential robot motion," "single-channel arithmetic display") at design time. The domain MUST NOT be general-purpose.

**1.2** The system MUST ship every tool required to go from empty state to a complete artifact within the domain, without external dependency resolution. No package manager, no linker, no third-party asset pipeline.

- *Anti-pattern:* a Python game framework that permits `pip install` for missing capability. The moment external resolution is sanctioned as the normal path (not an escape hatch — see §5), domain closure is broken and the system is a toolchain, not a kit.
- *Compliant:* PICO-8 ships sprite editor, tracker, code editor, and runtime in one cartridge format. STOS ships sprite/music/BASIC bound to one target (Atari ST).

**1.3** Rationale / trade-off: domain closure trades expressive range for zero integration failure. This is the correct trade when the goal is completion rate and legibility; it is the wrong trade when the goal is production capability. Do not apply this spec to tools whose success metric is "can build anything" — that is a different artifact class with different acceptance criteria.

---

## 2. Bounded State Space (MUST)

**2.1** The total reachable state space MUST be small enough for a single user to hold a complete mental model of it. This is a cognitive-load requirement, not merely a memory/storage requirement.

**2.2** Concretely, the system MUST enforce at least one hard resource ceiling: instruction count, program length, palette size, resolution, buffer depth, or component count. The ceiling MUST be enforced by the runtime/hardware, not by documentation or convention alone.

- Enforcement mechanism examples: PICO-8's 8192-token compile-time limit; Big Trak's 16-step physical buffer; Mattel Football's fixed LED matrix.
- **2.2.1** A ceiling enforced only by convention (e.g., "STOS games are typically small because that's what the community does") is a SOFT constraint. Systems with only soft constraints are marginal cases — see §6 (Boundary Cases).

**2.3** Testing implication: a conformance test for this property is direct — attempt to enumerate the full state space programmatically. If enumeration is computationally intractable for a single engineer to reason about by hand, the spec is violated regardless of what the marketing claims. This is the same test you'd apply to verify an API's surface area is actually minimal rather than nominally minimal.

---

## 3. Zero-Latency Actuation Loop (MUST)

**3.1** The interval between an authoring action (keypress, wire placement, line of code) and an observable, unambiguous output MUST be perceptible as instantaneous — no build step, no compile-link-run cycle, no deployment.

**3.2** The output signal MUST be unambiguous: the user must be able to attribute the observed change to the specific input action with no intermediate inference step.

**3.3** Failure mode to avoid: any toolchain stage that defers feedback (batch compilation, CI pipelines, async deploy) violates this clause even if the total latency is objectively short. Perceived latency and step-count matter more than wall-clock time — a 2-second compile with a visible progress bar reads as "waiting"; a REPL with 2-second GC pause reads as "broken." Design for the former by eliminating the step, not by optimizing it.

---

## 4. State Legibility (MUST)

**4.1** Internal state MUST be inspectable by the user without instrumentation. Encapsulation, in the software-engineering sense, is a design smell in this domain, not a virtue.

**4.2** The complete program/circuit/configuration SHOULD be viewable in a single frame (one screen, one board, one card) without scrolling, paging, or file-switching. This is a stronger requirement than "the API is well-documented" — it is "the whole thing fits in your visual field."

**4.3** Architectural consequence: this clause is in direct tension with clause 1 of most production software style guides (hide implementation detail behind interfaces). Do not import general software engineering "good practice" into toy program design uncritically — information hiding that is good practice in a 500KLOC codebase is a defect here, because the target audience's task is building a causal model, not managing complexity at scale. Know which regime you're in before applying a heuristic from the other one.

---

## 5. Escape Hatches (SHOULD, with constraints)

**5.1** The system SHOULD provide at least one sanctioned mechanism for advanced users to exceed the default abstraction without altering the default contract for all other users.

- Examples: `peek()`/`poke()` in PICO-8; STOS's raw memory access; Big Trak's chained-run trick (re-triggering GO to exceed the 16-step ceiling).

**5.2** An escape hatch MUST NOT become the primary interface. If a majority of real-world usage routes through the escape hatch rather than the default abstraction, the abstraction has failed and clause 1.2 (domain closure) is being silently violated via the back door. Treat this as a design regression, not a feature success — it indicates the default toolset can't service its own stated domain.

**5.3** Testing implication: track escape-hatch usage as a metric, the same way you'd track fallback-path invocation in a production system. High fallback usage is a signal the primary path is under-specified, regardless of domain.

---

## 6. Failure Reversibility (MUST)

**6.1** No user action reachable through the normal interface may cause unrecoverable state loss, hardware damage, or a crash that discards more than the current editing session.

**6.2** Error signals MUST be immediate, local, and low-ceremony. A line-numbered rejection is compliant; a stack trace requiring symbol resolution is not, regardless of how much information it technically conveys.

**6.3** STOS is a partial violation here on period hardware — an infinite loop could hang the machine and lose unsaved work, which is a stronger failure mode than PICO-8's contained crash-back-to-editor or Big Trak's "drives into the wall and beeps." This is a legitimate spec violation, not a stylistic quirk, and it's the primary reason STOS sits at the boundary of this category rather than solidly inside it. Log this as a known deviation rather than rationalizing it away — the spec should be falsifiable, and STOS is the falsifying case worth keeping visible.

---

## 7. Undiscovered Ceiling (SHOULD)

**7.1** The system SHOULD have a mastery ceiling that is not disclosed at onboarding. Advanced technique should be discoverable through continued use, not front-loaded in documentation.

**7.2** The resource ceiling from §2.2 SHOULD double as a design puzzle: the constraint itself should be a target users optimize against voluntarily (demoscene-style competition), not merely an obstacle they route around. If users treat the ceiling purely as friction to be minimized, the constraint has failed to become generative — this is a signal to re-examine whether the ceiling is at the right level, not a call to remove it.

---

## 8. Portability / Serialization Boundary (SHOULD)

**8.1** A complete artifact (program, circuit, cartridge) SHOULD serialize to a single self-contained unit transferable between users without environment reconstruction.

**8.2** This clause is what elevates individual mastery (§7) into a compounding community effect. A `.p8` cart, a wiring diagram, and a magazine BASIC listing are all instances of this — the serialization boundary matches the domain-closure boundary (§1), so nothing is lost or requires re-assembly on transfer.

**8.3** Architectural note: this is the toy-program analog of a stable wire format in distributed systems — the value isn't the format itself, it's that the format boundary exactly matches the domain boundary, so no out-of-band context is required to interpret a transferred artifact.

---

## 6b. Conformance Table (informative)

| Clause | Mattel Football | 150-in-1 | Big Trak | STOS | PICO-8 |
|---|---|---|---|---|---|
| 1. Domain closure | PASS (hw-forced) | PASS | PASS | PASS | PASS |
| 2. Bounded state (hard) | PASS | PASS | PASS | **SOFT** | PASS |
| 3. Zero-latency loop | PASS | PASS | PASS | PASS | PASS |
| 4. State legibility | PASS | PASS | PASS | PASS | PASS |
| 5. Escape hatch discipline | N/A | N/A | PASS | PASS | PASS |
| 6. Failure reversibility | PASS | PASS | PASS | **PARTIAL** | PASS |
| 7. Undiscovered ceiling | N/A (too small) | PASS | PASS | PASS | PASS |
| 8. Portability | N/A | PASS | N/A | PASS | PASS |

STOS is the only item with two non-PASS cells, both stemming from the same root cause: its constraint is domain-level and cultural (§1, §7 hold), but its enforcement is not hardware-backed (§2, §6 degrade). That is a real, falsifiable weakness in classifying STOS as a full toy program rather than a permissive dialect used toy-program-style by convention.

---

## 9. Non-Goals

This spec explicitly does NOT optimize for: expressive power, production readiness, scalability, or team collaboration. A system that satisfies this spec is expected to lose to a general-purpose toolchain on all four axes. That loss is the correct trade-off for this artifact class, not a defect to be fixed. Do not "improve" a compliant toy program by relaxing §1 or §2 — that produces a worse toolchain, not a better toy.