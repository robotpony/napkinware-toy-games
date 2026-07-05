# Ideas.md

Toy programs teach, inspire, and entertain.

## Naming scheme

The collection is called **Napkinware**. Individual toys follow the pattern
**Napkinware Kit ##: Title**, a mashup of RadioShack's numbered project kits
(150-in-1 Electronic Project Kit) and Usborne's playful, literal children's
book titles (Machine Code for Beginners, Computer Spacegames You Can Play).

Working titles, one per category below:

| Kit | Category | Title | Alternates |
|-----|----------|-------|------------|
| 01 | Computing principles (CPU basics) | The Bit Factory | Meet the CPU, How It Adds Up |
| 02 | Computed art | Paint by Logic | Draw Yourself a Program, The Pixel Loom |
| 03 | Computed sound design | The Wave Machine | Tune the Machine, Noise Kit |
| 04 | Computing history | Before the Screen | Switches, Cards, Terminals; The Museum in a Box |
| 05 | Video game basics | Sprites & Loops | Game in a Box, Bounce, Collide, Repeat |
| 06 | Logic gates (new) | Before the Bit Factory | Gates & Switches, The Switch Box |
| 07 | Cellular automata / emergent systems (new) | Rules of Life | Little Rules, Big Patterns; The Grid That Grows |
| 08 | Cryptography and codes (new) | Secrets & Ciphers | The Code Room, Breaking the Code |
| 09 | Analog electronics (new) | The Component Box | Circuits & Signals, Wires & Waves |
| 10 | Algorithms as spectacle (new) | Sorting Things Out | Race to Sorted, The Sorting Yard |
| 11 | Competitive programs (new) | Code Versus Code | The Arena, Program Eat Program |

Open questions:
- Whether every kit title should share one grammatical pattern (all "The
  ___ Machine/Factory," or all noun-phrase pairs like "Sprites & Loops"),
  or whether a mixed bag across the shelf is more in the Usborne spirit,
  where each book title did its own thing.
- Kit 06 (logic gates) is a prerequisite to Kit 01 (CPU basics)
  conceptually, but numbering treats 01 as first. Decide whether the
  Kit ## sequence should reflect build order, learning order, or
  release order; those aren't the same ordering here.
- Kits 03 (sound) and 09 (analog electronics) overlap at the
  oscillator/waveform level. Decide whether analog electronics is its
  own kit or folds into computed sound design as a prerequisite section.

## Computed art

Toys that teach art through computer control: generative pattern-making,
fractals, and painterly systems, not just Logo-style turtle graphics.

Prior art: Harold Cohen's AARON (the original "computer as artist"
program, arguably the namesake precedent for this category), Vera
Molnár's plotter drawings, the Processing/p5.js community (Casey Reas,
Ben Fry).

**Kit 02: Paint by Logic**

## Computed sound design

Toys that teach music through logic, including sound design and
sequencing, special effects, and music theory.

Prior art: Sonic Pi (live-coded music), chiptune/tracker culture
(including PICO-8's own tracker), modular synth panels (Make Noise,
Buchla) as a hardware analog to the same waveform concepts.

**Kit 03: The Wave Machine**

## Computing principles

**Kit 01: The Bit Factory**

1. CPUs
   1. How they work
   2. Types of CPUs
   3. Bus widths
2. Number representation
   1. Binary and hex
   2. Two's complement
   3. Floating point
3. Memory and storage
   1. Registers vs. RAM vs. disk
   2. How data persists between runs
4. Agents as computing
   1. botnets
   2. communications
5. Communication protocols

See also Kit 06 (logic gates), the layer below CPUs.

## Computing history

**Kit 04: Before the Screen**

1. Programming evolution
   1. Switches
   2. Punch cards
   3. Terminals
2. The people
   1. Ada Lovelace
   2. The ENIAC programmers
   3. Grace Hopper
3. The personal computer moment
   1. Altair, Apple I, homebrew clubs
4. The GUI and mouse (Xerox PARC)

## Video game basics

**Kit 05: Sprites & Loops**

1. Evolution
   1. raster
   2. vectors
   3. sprites
   4. 3d
2. simple game loops
3. input handling
4. collision detection
5. DSLs

## Computing versus hardware

1. Connecting to the outside world

## Logic gates (new)

Toys that build up from switches to AND/OR/NOT to a simple adder, before
any CPU or registers exist. Sits conceptually before Kit 01, not after
it: this is the layer most of the project's own reference models
(Turing Tumble-style logic, 150-in-1's switch circuits) actually live
in, but IDEAS.md had no toy here until now.

Prior art: Turing Tumble (physical marble-logic puzzle, no code at
all), nandgame.com (build a CPU from NAND gates in-browser), MHRD
(Steam game with the same premise).

**Kit 06: Before the Bit Factory**

## Cellular automata / emergent systems (new)

Toys built on a grid and a rule table: simple local rules producing
complex global behaviour. Strong fit for state legibility (the whole
system is visible on one screen) and bounded state (grid size is the
ceiling).

Prior art: Conway's Game of Life, Wolfram's elementary cellular
automata, Craig Reynolds' boids/flocking, Nicky Case's *Parable of the
Polygons*.

**Kit 07: Rules of Life**

## Cryptography and codes (new)

Toys that teach codes and ciphers, starting mechanical (Caesar and
Vigenère ciphers) before going electromechanical (Enigma). Bridges
computing history and computing principles.

Prior art: physical Enigma replicas, Simon Singh's *The Code Book*
puzzles.

**Kit 08: Secrets & Ciphers**

## Analog electronics (new)

Toys that teach resistors, capacitors, and oscillators directly, the
way the 150-in-1 kit did. Currently the only reference model in
CONSTRAINTS.md with no corresponding toy in this file.

Prior art: the RadioShack 150-in-1 kit itself, Make Noise/Buchla-style
modular synth panels as a contemporary analog.

**Kit 09: The Component Box**

## Algorithms as spectacle (new)

Sorting and searching as visual, audible performance rather than
abstract complexity theory. Satisfies domain closure and bounded state
cleanly: fixed array size, one screen, instant feedback per swap.

Prior art: *Sorting Out Sorting* (1981 NFB animated film, arguably the
original toy-program-as-teaching-tool), "sound of sorting"
audibilizations, visualgo.net.

**Kit 10: Sorting Things Out**

## Competitive programs (new)

Small programs competing against each other in a shared arena, rather
than a single user's program running alone. Satisfies the spec closely:
closed domain, tiny instruction set, one arena, instant feedback, and
an escape-hatch culture (redcode optimization as sport) that matches
the undiscovered-ceiling clause almost exactly.

Prior art: Core War / Redcode (1984).

**Kit 11: Code Versus Code**
