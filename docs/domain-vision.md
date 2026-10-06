# Domain Vision Statement — tiferet-hls

**Status:** Draft · **Domain:** `hls` · **Code:** `tiferet_hls/` (intended; not seeded) · **Branch:** `docs-domain-vision`

## The bet: a library of architectures, not a pile of copies

The usual way to compare hardware designs is to copy a folder: one kernel, one script, one tool project, edited by hand. What makes one version different is a script diff. How it did is whatever the tool left on disk. Move the copies and the paths break. That wrings rows out of one kernel. It does not manage a library under real conditions: the part, the clock, the operators, and a later run of the same design.

tiferet-hls takes the opposite position. **A study is a declared project:** a list of designs, each a code snippet plus the hardware structure mapped onto it. That structure is the adders, multipliers, multiplexers, and clocks, and the directives bound to labels the snippet already has. The folder the tool runs in is compiled from the project into one archive. Someone else runs the script in that archive. The reports come back as a second archive and attach to the design that produced them.

## What this domain makes real

tiferet-hls is the record and the round trip. It holds snippets, designs, and projects. A structure can be drawn for one design or taken from a template. A snippet can be copied, or promoted to a template, without rewriting the design it came from. The export holds a generated snippet per design, the tool scripts, and a shell script that runs synthesis and packs the reports and the log. A design can be recorded before it has been run. A second run adds a measurement. It does not become a different design.

An author may start from a snippet alone. Compiling it into ElohaSL, the declared form owned by tiferet-elsl, may propose a baseline structure. That proposal is not the design until it passes the same check as one drawn by hand.

## What we get for it

### A library instead of a case statement

Knowing which result belongs to which architecture, once the folders multiply and the same design is run again under a different part or clock, is the costly part. The project already knows: the design was named, and the measurement attached to that name. A further design is a declaration, not another copy.

### Code and architecture that are not allowed to drift

The snippet and the hardware structure are different records. A design that maps an operator, a clock, or a directive onto a label the snippet does not have is refused before export. It should fail here, not on a server, and not by renaming a label so that it appears to run.

### One archive out, one archive back

The export is the whole project. Paths are part of it, so moving it does not break the build. The return archive is all the application needs in order to read the run. The application does not call the synthesis tool. The script does, on a machine this record does not own.

### Numbers that keep their provenance

Latency, resource counts, and the achieved interval — how often a pipelined loop can accept new data, often visible only in the run log — attach to the design they came from. Throughput and area are computed from those and marked as computed. A formula is recorded only if the author states it. A figure, or a table of the same numbers, is made from that record. It is not a second one.

## The core of the work

> **Record** the snippet and its labels → **bind** a hardware structure and its directives to those labels → **check** that the two agree → **export** an archive a script can run → **read** the archive that script returns.

What varies is the snippet, the labels the author names on it, the structure mapped onto those labels, the setup of the run (part and clock, not an optimization), and how many times a design has been measured: none, once, or again later.

The commitment is: **the design is the fixed point; the emitted files, the tool that runs them, and the measurements are the variable ends.** The tutorial matrix–vector kernel is the first fixture, not the product.

## What it deliberately does not do

It does not synthesize. The script does. The server, the tool version, and the license stay outside the record. It does not pick a winner. It does not draw the figure; tiferet-plot owns the picture.

It does not own the hardware vocabulary. Adders, multipliers, multiplexers, clocks, and their templates belong to tiferet-fpga. This domain holds the mapping onto a design's labels. It does not own ElohaSL. tiferet-elsl compiles a snippet into that form. Turning that form back into C synthesis code lives here, because no other tool needs it. Choosing a structure from runtime state is tiferet-mlir, and it is later.

It does not invent labels by reading C and renaming them. A proposed baseline that does not match is a finding, not a fix. A click-built canvas is not this cut, and tiferet-elements is not that canvas. A management screen is later work.

---

*Companion document:* `docs/core-domain-distillation.md` — the detailed
walkthrough of the domain's vocabulary, behaviors, and the relationships
between its parts.
