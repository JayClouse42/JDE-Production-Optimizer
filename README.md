# JDE-Production-Optimizer
Flag ship Ophidian Mechanics Production Planning Optimization with Schlangenkraft Engine
Code name: Frobozz Magic OP

[listen to Lambda-Chan's song](https://www.youtube.com/watch?v=NvzGXr3trZs)

[Demo on Youtube](https://www.youtube.com/watch?v=H19CVI6TE-Q)

A production-feasibility optimizer for JD Edwards — powered by the OP-1 Schlangenkraft engine.

Turn a 90 day range ERP snapshot into an honest, constraint-respecting production plan — and see exactly *why* every work order can or can't run.

> **Portfolio demo.** This repository hosts a runnable, offline build that ships with a fully **de-identified** dataset — every item, work order, and description is replaced with a generic placeholder, so no real names or numbers are exposed. No database, no credentials, nothing to install. It exists to show the tool end-to-end. The engine source is kept private.

## The problem

Manufacturers running JD Edwards can pull what they *have* — on-hand inventory, open POs, work orders, bills of material — but not the answer they actually need:

> Given today's real constraints, what can we actually build, and what's stopping the rest?

Spreadsheets and stock MRP tend to hand back an optimistic plan that fails on the floor: they pool inventory across plants that can't share it, ignore whole-batch minimums, and assume a component is available the moment it's ordered. Frobozz refuses to lie about any of that.

## What it does

Frobozz reads a read-only snapshot (JD Edwards via Snowflake), explodes every bill of material, assembles time-phased supply pools, and runs a three-stage solver that respects the realities that make planning hard:

- **Material balance** — strict conservation; a case is never spent twice (Kirchhoff).
- **The weakest-link law** — a work order is capped by its scarcest ingredient (Liebig); miss any one BOM line and it produces nothing.
- **Whole batches** — production is indivisible; you can't make half a batch.
- **Plant boundaries** — a work order consumes stock only at a plant it sources from; inventory anywhere else is stranded — a transfer candidate, not a purchase.
- **Lots & expiration** — on-hand is a set of dated lots, drawn earliest-expiry-first (FEFO).
- **Time** — a PO that lands after a work order starts is no help to it.
- **Demand priority** — firm sales orders outrank safety-stock, which outranks forecast.

The output is a feasible plan plus a full, honest explanation of every shortfall.

## ▶️ Try the demo (Windows)

Download the demo build from the **Releases** page and run it — no install, no database, no credentials.

- **Option 2** opens the interactive viewer — it launches your browser automatically at **http://localhost:8766** and loads the full solved plan (with every identifier already scrubbed).
- **Option 3** re-runs the solver on a self-contained synthetic universe, so you can watch the engine work end-to-end.

The demo dataset carries a complete universe — finished goods, multi-level WIP, raws, packaging, plants, and dated lots — engineered to exercise every feature, with all identifiers de-identified.

## Features

### 🕸️ Interactive supply-chain graph
The whole plan as a colored network — green / amber / red by feasibility, edge width by flow. Click any node for feasibility, planned-vs-feasible, and a per-component diagnosis: is it supply-bound, out-competed for a shared pool, arriving too late, or stranded at the wrong plant? On-hand by plant with a reachable-vs-stranded split, the FEFO lot list, and a time-phased run-out timeline. Every ingredient edge shows its own competition rank — *#2 of 5 competing*, how much of the pool remained when this work order drew, and how much it got versus needed. **Pegging** — what fills each order, when, and from which plant and lot.

### 🔀 What-if analysis (non-destructive)
- **Cut a work order** and watch the cascade — freed components re-flow to other orders, and orphaned supply surfaces.
- **"What can I cut to help this order?"** — ranked suggestions with the fill gained per unit cut.
- **Force-available** — presume a component unlimited and re-solve to reveal what it was really blocking.
- **Fire a ghost** — turn a planned-but-not-yet-released work order on.
- **Bake to graph** — accept a scenario and make it the working plan, without corrupting the before/after math.

### 🧠 Brain analytics — network analysis of the solved plan
Treat the finished plan as a network and study its structure — the same viewer, the same solved plan, no re-solve.

- **Influence clusters** — groups of finished goods coupled through shared *binding* constraints. Change one and see which others actually move, and which upstream scarcity links them. Communities come from modularity maximization (Louvain), so the groupings reflect real coupling — not "everything that happens to use molasses."
- **Flow web + cut cross-impact** — a rotatable 3-D web of the shared-component chains between finished goods, with the WIP work orders shown as the routing junctions. Cut one finished good and trace the cascade: supply that redirects *forward* to another order (green), supply released *backward* to WIP or on-hand (red), and anything left stranded, surfaced as an orphan.
- **Structure measures** — community modularity; cliques and the dense k-core of mutually-competing orders; spectral analysis (normalized Laplacian, Fiedler vector and algebraic connectivity — how nearly the plan splits in two); 0-dimensional persistent homology (how the plan fragments as you filter edges by constraint strength); and lesion analysis (remove a node and measure its downstream kill-closure under the weakest-link law).

### 🎯 Objective modes
Solve for **fill rate** (units shipped), **high value** (revenue-weighted), or **profit** (margin-weighted) — the same feasible plan, steered toward what matters this week. Plus a division filter and a lead-planner control panel with tunable solver weights.

### 📊 Reports
Excel exports (production plan, bottleneck $-leverage, run-out) plus on-screen cut analysis and a what-if audit log.

## How it works

Powered by the **Schlangenkraft** engine under **Ophidian Mechanics**. A proprietary multi-stage process discovers true material pressures and produces a discrete, BOM-exact feasible plan that respects real constraints. The system always computes the honest answer first, then displays it.

## Tech stack

Python · NumPy / SciPy · pandas · NetworkX + python-louvain for the network-analysis layer · a custom Phoenix Tempering selector and Schlangenkraft engine pricer · Flask graph API · a self-contained HTML5 Canvas viewer · Snowflake connector (live mode) · packaged to a single Windows `.exe` with PyInstaller.

## Status & disclaimer

Pre-release demo — not production-ready. Built for a real production-planning team running JD Edwards on Snowflake; this public build runs entirely on de-identified data, so numbers and item names are placeholders. Live data surfaces edge cases the demo won't.

Frobozz and the general aura of interactive-fiction magic are affectionate homages to the classic text adventures. No grues were harmed.


Status & disclaimer
Pre‑release demo — not production‑ready. Built for a real production‑planning team running JD Edwards on Snowflake; this public build runs entirely on synthetic data, so numbers and item names are invented. Live data surfaces edge cases the demo won't.

Frobozz, xyzzy, and the general aura of interactive‑fiction magic are affectionate homages to the classic text adventures. No grues were harmed.
