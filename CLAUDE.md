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

## Commands

No build system exists yet. Commands will be added here once the project is scaffolded in Phase 4.
