# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

This is a collection of educational "toy programs" — self-contained interactive experiences that teach, inspire, and entertain (in that order). The reference models are PICO-8, Big Trak, Mattel Football, and the 150-in-1 electronics kit.

Every toy MUST conform to the constraints defined in `CONSTRAINTS.md`. Read that file before proposing any design. The key invariants are: domain closure (no external dependency resolution at runtime), bounded state space (a user can hold the whole thing in their head), zero-latency actuation (no build steps between action and feedback), and state legibility (internals visible without instrumentation).

## Current phase

**Exploration.** No code exists yet. The current tasks are:
1. Evaluate Godot as the platform (primary candidate) against the CONSTRAINTS.md spec
2. Design the shared framework architecture (teaching layer + toy layer)
3. Scope the first toy: **CPU basics** (how CPUs work, bus widths, registers — a visual simulator)

Platform direction: **Godot** (open source, cross-platform, GDScript or C#, strong 2D support).

## Planned phases

1. **Exploration** — platform evaluation, first toy scoping (current)
2. **Product specs** — PRD for the shared framework and CPU basics toy
3. **Architecture** — package/module structure, shared teaching layer, cross-toy communication model
4. **Package development** — scaffolding and first toy implementation

## Architecture intent (from README)

Two layers of shared code are planned:

- **Teaching layer**: Handles instructional content (markdown-based, common folder structure, analogous to Ren'Py but for teaching). This layer is domain-agnostic across toys.
- **Toy layer**: Handles shared UX, networking capabilities, visualization styling, and interaction patterns that are consistent across all toys.

Each toy is fully self-contained and can stand alone, but MAY communicate with other toys in the collection.

## Key files

- `CONSTRAINTS.md` — formal spec (MUST/SHOULD/MAY) for what qualifies as a toy program. Read this before any design work.
- `ARCHITECTURE.md` — platform decision (Godot 4, GDScript only), two-layer structure, distribution targets, and constraint compliance responsibilities.
- `IDEAS.md` — candidate toy categories: computed art, computed sound design, computing principles, computing history, video game basics.
- `README.md` — goals and architectural considerations.

## Commands

No build system exists yet. Commands will be added here as the project is scaffolded.
