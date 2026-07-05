# Architecture

## Platform decision: Godot 4, GDScript only

**Platform:** Godot 4 (current stable: 4.6.x).

**Scripting:** GDScript exclusively. No C#. This is a hard constraint, not a preference — C# cannot target the web export, and the web export is a first-class distribution target. Mixing languages breaks this permanently; commit to GDScript now.

**Evaluated alternatives:**
- Pygame: good fit for simulation-heavy toys, weaker on distribution and UI tooling.
- Web (Phaser/Canvas): best portability story, weaker for anything that needs to feel like physical hardware. Consider for future toys if the audience skews browser-first.
- Unity: disqualified by compile-step friction (violates the development-time intent of §3), Package Manager as default workflow (§1.2 anti-pattern), and licensing trust risk.

---

## Two-layer architecture

Every toy is built from two shared layers, instantiated together in a common shell scene.

### Teaching layer

Handles all instructional content. Implemented as a reusable Godot scene (not per-toy logic).

- Content is authored in Markdown and rendered via `RichTextLabel` with BBCode conversion (Godot 4.6 improved Markdown-to-BBCode support).
- Follows a common folder structure across all toys so content can be found and edited without touching engine scenes.
- Analogous to Ren'Py's scripted narrative layer, but for interactive teaching rather than story.
- Placement, icons, and interaction patterns are shared across all toys (README goal 9).

### Toy layer

Handles the interactive simulation and shared UX. Also a reusable scene, extended per toy.

- Common networking capabilities (WebSocket/WebRTC — the only options available in web exports).
- Common visualization styles, UI chrome, and interaction patterns.
- Each toy extends this layer; it does not override it.

---

## Toy structure

Each toy is a fully self-contained Godot scene tree. It can be run standalone or loaded into the shared shell.

Toys may communicate with each other (README goal 4), but must stand alone on features. Inter-toy communication, where it exists, uses the networking layer in the shared toy base — not direct scene references.

Toys progress conceptually (e.g., CPU basics → serial protocols → storage), but this is a content relationship, not a code dependency. A later toy may reference concepts from an earlier one in its teaching layer content, not in its GDScript.

---

## Distribution

**Primary target: desktop.** Self-contained ZIP — no installer, no admin rights, no runtime dependency. Godot's export templates produce a single executable + data file.

**Secondary target: web (HTML5).** Compatibility renderer (WebGL 2.0). Constraints:
- Keep each toy's asset payload under ~20MB.
- Threaded builds require specific HTTP headers (`Cross-Origin-Opener-Policy`, `Cross-Origin-Embedder-Policy`). Itch.io and GitHub Pages handle this automatically; a bare S3 bucket does not.
- No TCP/UDP — inter-toy networking on web uses WebSocket only.

---

## Constraint compliance responsibilities

CONSTRAINTS.md defines what each toy must satisfy. Two clauses require active design effort — the platform does not provide them automatically.

**§2 Bounded state space:** Each toy must define and enforce at least one hard ceiling (instruction count, step buffer size, palette entries, etc.) at the GDScript level. Document the ceiling in the toy's own spec before building it. If you can't name the ceiling, the toy isn't ready to build.

**§6 Failure reversibility:** Godot games can crash or reach invalid states. Every toy must implement an explicit "return to safe state" path reachable from the normal UI — not just from a restart. Design this before building the happy path.

---

## State legibility convention

§4 requires internal state to be inspectable without instrumentation. In Godot terms: the simulation's internal state (registers, buffers, counters, signals) is rendered as first-class UI elements, not read from the debugger. If the user has to open the Godot remote debugger to understand what the toy is doing, the toy is not compliant.

This is an architectural convention enforced by code review, not by the engine.
