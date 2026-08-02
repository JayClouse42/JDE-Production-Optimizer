# JDE-Production-Optimizer
Flag ship Ophidian Mechanics Production Planning Optimization with Schlangenkraft Engine
Code name: Frobozz Magic OP

[listen to Lambda-Chan's song](https://suno.com/song/8d809306-def7-440b-8df2-e4c05ba2c881)

[Demo on Youtube](https://www.youtube.com/watch?v=H19CVI6TE-Q)

A production-feasibility optimizer for JD Edwards — powered by the OP‑1 Schlangenkraft engine.

Turn a single day's ERP snapshot into an honest, constraint‑respecting production plan — and see exactly why every work order can or can't run.

Platform Built with Demo Status

Portfolio demo. This repository hosts a runnable, offline build that ships with synthetic test data — no real business data, no database, no credentials required. It exists to show the tool end‑to‑end. The engine source is kept private.

The problem
Manufacturers running JD Edwards can pull what they have — on‑hand inventory, open POs, work orders, bills of material — but not the answer they actually need:

Given today's real constraints, what can we actually build, and what's stopping the rest?

Spreadsheets and stock MRP tend to hand back an optimistic plan that fails on the floor: they pool inventory across plants that can't share it, ignore whole‑batch minimums, and assume a component is available the moment it's ordered. Frobozz refuses to lie about any of that.

What it does
Frobozz reads a read‑only snapshot (JD Edwards via Snowflake), explodes every bill of material, assembles time‑phased supply pools, and runs a three‑stage solver that respects the realities that make planning hard:

Material balance — strict conservation; a case is never spent twice (Kirchhoff).
The weakest‑link law — a work order is capped by its scarcest ingredient (Liebig); miss any one BOM line and it produces nothing.
Whole batches — production is indivisible; you can't make half a batch.
Plant boundaries — a work order consumes stock only at a plant it sources from; inventory anywhere else is stranded — a transfer candidate, not a purchase.
Lots & expiration — on‑hand is a set of dated lots, drawn earliest‑expiry‑first (FEFO).
Time — a PO that lands after a work order starts is no help to it.
Demand priority — firm sales orders outrank safety‑stock, which outranks forecast.
The output is a feasible plan plus a full, honest explanation of every shortfall.

▶️ Try the demo (Windows)
Download FrobozzMagicOP_offline.exe from the Releases page.
Run it. It generates a synthetic dataset and solves it — no install, no database, no credentials.
Choose option 1 to build the plan (pick the Offline data source when prompted).
Choose option 2 to open the interactive viewer — it launches your browser at http://localhost:8765 automatically.
The offline build carries its own fabricated universe — finished goods, multi‑level WIP, raws, packaging, plants, and dated lots — engineered to exercise every feature end‑to‑end.

Features
🕸️ Interactive supply‑chain graph
The whole plan as a colored network — green / amber / red by feasibility, edge width by flow.
Click any node for feasibility, planned‑vs‑feasible, and a per‑component diagnosis: is it supply‑bound, out‑competed for a shared pool, arriving too late, or stranded at the wrong plant?
On‑hand by plant with a reachable‑vs‑stranded split, the FEFO lot list, and a time‑phased run‑out timeline.
Pegging — what fills each order, when, and from which plant and lot.
🔀 What‑if analysis (non‑destructive)
Cut a work order and watch the cascade — freed components re‑flow to other orders, and orphaned supply surfaces.
"What can I cut to help this order?" — ranked suggestions with the fill gained per unit cut.
Force‑available — presume a component unlimited and re‑solve to reveal what it was really blocking.
Fire a ghost — turn a planned‑but‑not‑yet‑released work order on.
Bake to graph — accept a scenario and make it the working plan, without corrupting the before/after math.
🎯 Objective modes
Solve for fill rate (units shipped), high value (revenue‑weighted), or profit (margin‑weighted) — the same feasible plan, steered toward what matters this week. Plus a division filter and a lead‑planner control panel with tunable solver weights.

📊 Reports
Excel exports (production plan, bottleneck $‑leverage, run‑out) plus on‑screen cut analysis and a what‑if audit log.

How it works
Powered by the Schlangenkraft engine under Ophidian Mechanics.
A proprietary multi-stage process discovers true material pressures and produces a discrete, BOM-exact feasible plan that respects real constraints.
The system always computes the honest answer first, then displays it.

Tech stack
Python · NumPy / SciPy · pandas · a custom Pheonix Tempering selector and Schlangenkraft engine pricer · Flask graph API · a self‑contained HTML5 Canvas viewer · Snowflake connector (live mode) · packaged to a single Windows .exe with PyInstaller.

🪄 There's a wizard in here
Type xyzzy in the viewer. You've been warned.

Status & disclaimer
Pre‑release demo — not production‑ready. Built for a real production‑planning team running JD Edwards on Snowflake; this public build runs entirely on synthetic data, so numbers and item names are invented. Live data surfaces edge cases the demo won't.

Frobozz, xyzzy, and the general aura of interactive‑fiction magic are affectionate homages to the classic text adventures. No grues were harmed.
