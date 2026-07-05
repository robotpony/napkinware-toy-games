# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A collection of educational "toy programs" — self-contained interactive experiences that teach, inspire, and entertain (in that order). Reference models: PICO-8, Big Trak, Mattel Football, the 150-in-1 electronics kit.

Every toy MUST conform to `docs/CONSTRAINTS.md`. Read it before proposing any design. Key invariants: domain closure, bounded state space, zero-latency actuation, state legibility.

## Platform

**Godot 4, GDScript only.** No C#. This is decided. See `docs/ARCHITECTURE.md` for rationale and the full two-layer architecture (teaching layer + toy layer).

## Key files

- `docs/CONSTRAINTS.md` — formal spec (MUST/SHOULD/MAY) every toy must satisfy
- `docs/ARCHITECTURE.md` — platform decision, two-layer structure, distribution targets, constraint compliance responsibilities
- `PLAN.md` — current phase, task status, and upcoming phases
- `docs/IDEAS.md` — candidate toy categories
- `docs/specs/shared/` — PRDs for the shared framework (teaching layer, toy layer, shell scene)
- `docs/specs/infrastructure/` — PRDs for cross-cutting systems bigger than shared UX plumbing (currently: toy-to-toy and hive connectivity)
- `docs/specs/toys/` — one PRD per toy, named `kit-NN-slug.md` to match the Kit ## scheme in `docs/IDEAS.md`

## Repository structure

Single Godot project (decided over the multi-project alternative: shared-code reuse is free and each toy still exports standalone via its own export preset). Once scaffolded in Phase 4:

```
project/
  project.godot
  shell/                # entry-point scene, instantiates shared layers
  shared/
    teaching_layer/
    toy_layer/
  infrastructure/
    networking/          # port contract + connector toy's reference implementation
  toys/
    kit_01_bit_factory/  # snake_case in code, kebab-case in docs
```

`docs/specs/shared/`, `docs/specs/infrastructure/`, and `docs/specs/toys/` mirror the three code folders one-to-one: a spec's folder tells you which code folder it governs.

Source is released publicly (see `docs/ARCHITECTURE.md` § Source availability): some components' code is a teaching artifact in its own right, not just an implementation detail. The networking infrastructure is the first such case.

## Commands

No build system exists yet. Commands will be added here once the project is scaffolded in Phase 4.
