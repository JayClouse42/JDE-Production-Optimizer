# Production Planning Optimizer

**A constraint-based planning engine that turns a tangle of orders, inventory, and lead times into an executable production plan — and, crucially, explains *why* every shortfall happens and the single best action to fix it.**

> A working demonstration built on synthetic data. The production version integrates with ERP systems (JD Edwards) for multi-level manufacturing.

---

## The problem it solves

Manufacturers run on MRP systems that explode a bill of materials and tell you *what* you're short — but not *why*, and not *what to do about it*. When a plan comes back at 73% fill, a planner is left to reverse-engineer the cause by hand across thousands of work orders, shared ingredients, purchase orders, and expiration dates. So they stop trusting the system and fall back on spreadsheets and gut feel.

This engine was built to close that gap: produce a demand-prioritized, feasibility-aware plan **and** make it legible enough that a planner trusts it instead of overriding it.

## What it does

- **Multi-level BOM explosion with shared-component contention.** Resolves finished goods down through every sub-assembly and raw material, correctly handling ingredients that multiple products compete for.
- **Time-phased feasibility.** Respects on-hand inventory, dated purchase-order and work-order arrivals, lead times, batch sizes, and lot expiration (FEFO) across a rolling 90-day horizon.
- **Demand-prioritized allocation.** When supply can't cover everything, it allocates by a tunable demand priority (firm orders → safety stock → forecast) instead of first-come-first-served.
- **Plain-language diagnostics — the core differentiator.** For every shortfall it identifies the *specific* root cause (no supply, out-competed by a higher-priority order, a purchase order that's too small or too late, product that expires before it can be used) and names the single most effective corrective action.
- **What-if analysis.** Re-solves the full plan under changes — expedite a PO, move a work order, transfer stock between plants — and shows the honest downstream effect, including what a cut really costs in lost vs. gained orders.
- **Interactive supply-chain graph.** A visual map of the production network where feasibility, bottlenecks, and competing demands are visible at a glance.

## How it's different from standard MRP

| Standard MRP / black-box optimizer | This engine |
|---|---|
| Tells you *what* is short | Tells you *why*, and *what to do* |
| Plan is a black box; planners override it | Every number traces to a legible cause |
| First-come allocation | Demand-weighted allocation you can tune |
| Static run | Live what-if re-solves with honest tradeoffs |

## A peek under the hood

The engine is a multi-stage optimizer: a global warm-start to avoid lopsided local solutions, a shadow-pricing stage that turns demand priority and scarcity into a consistent set of marginal values, and a settlement pass that lands on a feasible, whole-batch plan obeying strict material-conservation and minimum-constraint rules. A diagnostic layer sits on top and mirrors the final solved state back to the user in plain language.

It draws on established operations-research methods — network-flow optimization, Lagrangian-style relaxation, and discrete/combinatorial search — applied to the realities of a live manufacturing environment. *Implementation details are intentionally omitted; this public repository is a sanitized demonstration, not the production system.*

## What's in this repo

- A runnable demonstration on a **synthetic, anonymized** dataset (no real company data).
- The interactive viewer and sample outputs / screenshots.
- A short case study describing the approach and a proposed A/B methodology for validating it against a live baseline.

## Tech stack

Python (pandas, NumPy, SciPy, NetworkX), a Flask service layer, and an HTML/JavaScript interactive viewer. Designed to integrate with JD Edwards / data-warehouse back ends in production.

## Status — an honest note

This is a personal project and a working proof of concept, validated on synthetic data that mirrors real manufacturing structure. It has **not** been run as the system of record in live production — doing so would mean a controlled trial against the existing planning process, which the case study lays out. The claims here are about **design and capability**, not measured deployment savings.

## About

Built by **Jay Clouse** — supply chain analyst with ~10 years in manufacturing and distribution operations, ERP (JD Edwards, SAP), and applied optimization. The aim of this project: bring rigorous, proven planning methods into a field that still largely runs on spreadsheets — carefully, and legibly enough to earn a planner's trust.

📫 clousejay@gmail.com
