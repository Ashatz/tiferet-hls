# Domain Vision Statement — tiferet-hls

**Status:** Draft · **Domain:** `hls` · **Code:** `tiferet_hls/` (intended; not seeded) · **Branch:** `docs-domain-vision`

## The bet: declare the design, don't clone the script

A hardware design study compares one algorithm optimized several different ways. The usual way to run one version is to copy a directory: the algorithm's source, a build script, and a tool project, edited by hand for whatever optimization this version is testing. Five versions mean five copies. What makes version three different from version two is stated nowhere but the difference between two scripts, and how well it did is stated nowhere but whatever the tool left on disk, found by hand and transcribed into a spreadsheet. Move the copies into a shared folder and the build paths break.

tiferet-hls takes the opposite position. **The design is a declared record, and the folder the tool runs in is emitted from that record.** Saying which algorithm, which labeled places inside it, and which optimization applies to each one is cheaper than maintaining another copy of a script. Unlike a copy, the statement survives being moved, rerun, and compared.

## What this domain makes real

tiferet-hls is a catalog with one database behind it. It knows an algorithm as a record — its source files, the function that tops it, and the labeled places inside it an optimization can attach to. It knows a named combination of optimizations on those places, the isolated files that let that combination run without destroying another one, and the measurements that came back after someone ran it.

A combination can be recorded before it has ever been run. A measurement attaches to it afterward. Running it a second time adds a second measurement; it does not make it a different design.

## What we get for it

### A comparison you don't assemble by hand

The costly part of a five-design study is not the synthesis. It is knowing which result belongs to which optimization, once the results are scattered across folders under names only their author can decode. The catalog already knows: the combination was named before the run, and the measurement attached to that name.

### Numbers that keep their provenance

Latency, resource counts, and the achieved interval — how often a pipelined loop can actually accept new data, a number that often appears only in the run log and not in the summary report — are copied out of the run and attached to the combination they came from. Throughput and area are computed from those and marked as computed. If the author states the formula, the record carries the author's formula; the library does not invent one, and never stores a derived figure as though the tool had reported it.

### Runs that don't destroy each other

The tool's own project-open step erases the project it opens, so two designs cannot share one. Isolation is therefore part of the record's job: one folder, one script, one project per combination, with source paths that still resolve after the folder is moved.

### The record outlives the way it is written down

The same optimization is written as a tool script in one setting and as a source annotation in another. Which form gets emitted is a detail of the run, not of the design — the record states the optimization, and the emitted form renders it. The first cut may emit only one form.

### Change that stays on the record

A sixth design is a declaration, not a sixth copy. A combination that names a place its algorithm does not declare is caught as wrong, rather than quietly becoming a design that will not build.

## The core of the work

Everything this domain does follows one path:

> **Declare** the algorithm and the labeled places inside it → **combine** optimizations on those places under a name → **emit** files that run in isolation → **attach** the measurements that come back.

Four things vary. The algorithm. Its labeled places, which differ from algorithm to algorithm — a loop is one kind, an array is another. The kind of optimization, which do not share a parameter list: pipelining takes a requested interval, unrolling takes a factor, splitting an array takes a type, a factor, and a dimension. And how many times a combination has been measured — none, once, or again later.

The design commitment is: **the declared combination is the fixed point; the emitted form, the tool that runs it, and the measurements are the variable ends.** The tutorial matrix–vector kernel that motivated this work is the first fixture, not the domain.

The labeled places are named by the author, from labels the source already carries. Reading the source to discover them is the wrong bet: an optimization attached to a renamed label is a different design, and will not run.

## What it deliberately does not do

It does not synthesize; Vitis HLS does, and remains the tool. It does not invoke Vitis either — no runner in this cut, and the compute server, tool version, and license sit outside the record. It does not decide which design won; the author judges. It does not draw the figure; tiferet-plot owns the picture. It does not read the algorithm's source to find labels. It does not run general applications or wire their parts together — that is the Tiferet framework.

A runner, further optimization kinds, and a second emitted form are later work, named here so they are not quietly assumed.

---

*Companion document:* `docs/core-domain-distillation.md` — the detailed walkthrough of the domain's vocabulary, behaviors, and the relationships between its parts.
