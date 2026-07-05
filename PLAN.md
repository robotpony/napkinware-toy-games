# Plan

## Phase 1: Exploration — current

**Goal:** Confirm the platform, document architecture, scope the first toy.

- [x] Platform evaluated and decided: Godot 4, GDScript only
- [x] Architecture documented: two-layer scene structure, distribution targets, constraint compliance responsibilities
- [ ] Scope the CPU basics toy against CONSTRAINTS.md (define the domain, name the state ceiling, design the failure reversibility path)
- [ ] Build a throw-away Godot prototype: CPU register display, to validate zero-latency actuation feel before committing to the full build

## Phase 2: Product specs

**Goal:** Write specs for the shared framework and the first toy before any production code.

- [ ] Shared framework PRD (teaching layer content schema, toy layer shared UX, shell scene structure)
- [ ] CPU basics toy PRD (domain definition, hard state ceiling, failure reversibility design, success criteria)

## Phase 3: Architecture

**Goal:** Full project structure on paper before scaffolding.

- [ ] Godot project and scene hierarchy
- [ ] Teaching layer folder layout and content authoring workflow
- [ ] Cross-toy communication model (WebSocket-based for web compatibility)
- [ ] State serialization format (satisfies §8 portability)

## Phase 4: Package development

**Goal:** Build the shared framework and the CPU basics toy.

- [ ] Scaffold Godot project
- [ ] Implement teaching layer
- [ ] Implement CPU basics toy
