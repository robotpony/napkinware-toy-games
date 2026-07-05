# Shared framework PRD

## Summary

Every Napkinware toy is built from two shared Godot layers (teaching, toy) instantiated together in a common shell scene, per `docs/ARCHITECTURE.md`. This spec defines what those three pieces actually contain: the teaching layer's content schema, the toy layer's shared UX and constraint-compliance scaffolding, and the shell's wiring between them. It exists so the first toy (Kit 01) extends a defined contract instead of inventing one, and so every toy after it inherits the same content format and UX for free.

## Problem

No shared framework exists yet. If each toy built its own teaching content system, its own state-legibility display, and its own "return to safe state" path, three things would go wrong:

1. Content authors would face a different markdown format and folder convention per toy.
2. Constraint compliance (CONSTRAINTS.md §4 state legibility, §6 failure reversibility) would be re-solved, and re-reviewed, once per toy instead of once for the whole collection.
3. Toys would drift visually and behaviourally, breaking the "feels like one kit" experience that the Napkinware naming scheme and shared shell scene are meant to deliver.

## Solution

### Teaching layer

Renders instructional content alongside the toy, per ARCHITECTURE.md's `RichTextLabel` + Markdown-to-BBCode approach.

**Content schema:**
- Each toy owns a `content/` folder (see `docs/specs/toys/`) containing one markdown file per lesson, numbered for order: `01-intro.md`, `02-registers.md`, and so on.
- Each lesson file has a single required frontmatter field, `title`. No other frontmatter until a second toy's content proves more fields are needed. Don't add a schema for a case that doesn't exist yet.
- The teaching layer scene reads the `content/` folder at runtime and lists lessons in filename order. No manifest file to keep in sync separately.

**Shared UX:**
- Placement, icons, and navigation between lessons are identical across all toys (README goal 9). A toy's GDScript never overrides teaching layer layout, only supplies content.

### Toy layer

Handles the interactive simulation shell and the UX every toy shares.

**Provides:**
- A **state legibility panel**: a fixed UI region where a toy renders its internal state (registers, buffers, counters) as first-class elements. CONSTRAINTS.md §4 requires this; the toy layer reserves the screen space and layout convention, and each toy fills it with its own fields.
- A **safe-state reset affordance**: a single always-visible UI control (button or keybind) that returns the toy to its initial state. This satisfies CONSTRAINTS.md §6's requirement for a normal-UI recovery path reachable without a restart. Each toy implements a `reset_to_safe_state()` method; the toy layer supplies the button and calls it.
- The **port contract**: the minimal shared interface a toy implements to expose named connection points. Everything about how connections are made, discovered, and visualized (including cross-toy and hive/botnet communication) lives outside the shared framework, in `docs/specs/infrastructure/networking.md`, not here.

**Constraint:** each toy extends the toy layer's base scene; it does not override shared layout or UX. If a toy needs to override rather than extend, that's a signal the shared layer is wrong, not that the toy should route around it (the same logic as CONSTRAINTS.md §5.2 on escape hatches).

### Shell scene

The top-level scene that instantiates the teaching layer and toy layer together and loads a specific toy into the toy layer's extension point. Standalone export builds point their entry scene at the shell with one toy pre-loaded; a future multi-toy launcher (if one gets built) would point the shell at a toy selected at runtime.

## Success criteria

- A content author can add a new lesson to an existing toy by dropping a numbered markdown file into `content/`, with no GDScript or scene changes.
- A new toy can be scaffolded by extending the toy layer's base scene and implementing `reset_to_safe_state()`, without modifying shared code.
- Every toy's safe-state reset is reachable within one UI interaction from any state, verified by manual walkthrough per toy before it ships.
- Two toys loaded in the shell in sequence look and navigate identically at the teaching layer and shell chrome level; only the toy layer's simulation area differs.

## Scope

**In:**
- Teaching layer content schema (frontmatter, folder convention, load order)
- Toy layer's state legibility panel convention and safe-state reset contract
- Shell scene's role as instantiator and toy-loading point

**Out:**
- Exact Godot scene tree and node hierarchy inside `shared/teaching_layer/` and `shared/toy_layer/` (Phase 3: architecture)
- Connection mechanics, cross-toy and hive communication (`docs/specs/infrastructure/networking.md`)
- Kit 01 specific content or simulation design (`docs/specs/toys/kit-01-bit-factory.md`)
- Visual design system (colour, typography, iconography) beyond "shared across toys"; a separate design pass, not this spec

## Open questions

- Does the state legibility panel need a layout convention stronger than "reserved screen region" (e.g., a fixed grid of labeled value slots), or is that over-specifying before Kit 01 proves out what state actually needs displaying? Leaning toward deciding this during Kit 01's build, not here.
- Should `reset_to_safe_state()` be a hard contract (base class requires override) or a soft convention (documented, checked in review)? Godot's lack of abstract method enforcement in GDScript makes the hard version awkward; likely soft convention plus a review checklist item.
- Undiscovered ceiling (CONSTRAINTS.md §7) is entirely a per-toy design concern; this spec makes no shared-framework provision for it. Confirm that's correct once Kit 01 is scoped.
