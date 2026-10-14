<!-- solve-problem:4ff0795e-74f6-43b2-bc95-a6b8cefe3fc7 -->
# Heterogeneous discounting with batch capacity

A [Tangle Solve problem](https://solve.tangle.tools/problems/4ff0795e-74f6-43b2-bc95-a6b8cefe3fc7).

## What is the problem?

Consider an online scheduling setting. Time is divided into rounds. Jobs arrive over time (chosen by an adversary); job *i* has a size/processing requirement *sᵢ*, a deadline *dᵢ*, and a value that decays over time according to a discount function *fᵢ(t)* — its value if completed in batch/round *t* is *vᵢ · fᵢ(t)*, with *fᵢ* nonincreasing and reaching 0 at the deadline. In each round, the scheduler has a **batch capacity** *C* (total job size schedulable per round, modeling a block's gas/compute limit) and must irrevocably decide which pending jobs to execute before seeing future arrivals. The objective is to maximize total discounted value collected.

The 2024–2026 work of Gafni and Yaish (see references) analyzes the **common-discount, unit-service** case: all jobs have the same decay law and unit size. The open problem is the **heterogeneous** generalization:

1. Jobs of **different sizes** *sᵢ* competing for shared batch capacity (a knapsack-like constraint per round);
2. Jobs with **different discount laws** *fᵢ* (e.g., different decay rates or deadline structures).

Goal: tight (or any nontrivial) competitive ratios for deterministic (and randomized) online algorithms against the offline optimum that knows the full arrival sequence. Precise statements of which heterogeneity classes admit O(1)-competitive algorithms — and which force Ω(log) or polynomial ratios — are the target. Ambiguity note: the census does not pin down whether sizes are arbitrary reals or integers, nor the exact decay family (exponential? arbitrary nonincreasing?); these model choices should be stated explicitly in any solution.

## Why does it matter?

This directly models block building in blockchains (transactions of varying size with value decay / deadline urgency competing for block capacity) and compute-market/spot allocation of perishable capacity. Tight bounds would let designers reason about mechanism design for fee markets (e.g., is a greedy-by-discounted-density rule provably near-optimal?). It also completes the theory arc from single-resource, homogeneous-discount scheduling to realistic heterogeneous settings; without it, existing guarantees do not apply to any real deployed system.

## What's already known?

The anchor is Gafni & Yaish, *Online Packet Scheduling With Time Discounts* (arXiv:2402.08549), which establishes competitive algorithms and matching or near-matching lower bounds in the common-discount unit-size model — essentially showing that a single decay law with unit jobs admits constant competitive ratios (verify exact constants and whether randomized bounds are tight). Related literatures supply partial tools:

- **Online knapsack with deadlines / bounded capacity**: known competitive ratios for unit or divisible items; general sizes typically lose constant factors or require resource augmentation.
- **Perishable-job scheduling and expiring knapsacks**: known hardness when deadlines differ; often Θ(log) competitive ratios or no bounded ratio without extra assumptions.

Why current approaches fall short: (a) heterogeneity in decay laws breaks the exchange/monotonicity arguments that let a single threshold policy work; (b) job sizes introduce per-round knapsack packing, and adversarial arrivals can force any online policy into bad packings; (c) combining both is untrodden. No published tight bounds exist for the joint setting as of the census date (verify).

## What would count as solving it?

Any of the following, stated for an explicitly defined model:

- **Upper bound**: a deterministic (or randomized) online algorithm with proven competitive ratio α for jobs with arbitrary sizes ≤ C and discount functions from a stated class (e.g., exponential with heterogeneous rates), with α ideally constant.
- **Lower bound**: an adversarial arrival construction showing no online algorithm (possibly randomized, via Yao) achieves ratio better than β, ideally matching α.
- **Separation results**: identify heterogeneity axes (sizes vs. decay laws vs. deadlines) that alone preserve constant competitiveness and which combinations destroy it.
- **Matching upper/lower bound** (full solution): α = β.

A referee would accept a paper-quality proof with the model fully formalized (arrival model, adversary type, allowed discount class, integrality of sizes).

## How to start

1. Read Gafni & Yaish (arXiv:2402.08549) end-to-end; isolate exactly which lemmas use common discounts and unit sizes. These are your breakage points.
2. Solve a warm-up: heterogeneous exponential discounts, unit jobs. Extend threshold/potential arguments; check whether mixing decay rates collapses to a single worst-case rate.
3. Second warm-up: common discount, integral sizes with per-round knapsack. Try greedy by discounted value/size ratio; compare to online-knapsack techniques (threshold pricing à la Zhou–Chakrabarty–Lukose (verify)).
4. Main obstacle: adversary interleaves large near-full-deadline jobs with small high-value ones, forcing capacity fragmentation — design lower-bound instances around this to map the frontier before building algorithms.

## Key references

- A. Gafni, A. Yaish, *Online Packet Scheduling With Time Discounts*, arXiv:2402.08549 (2024). Establishes competitive algorithms and lower bounds for online scheduling of unit jobs whose value decays over time under a common discount; the direct predecessor model this problem generalizes. https://arxiv.org/abs/2402.08549
- Literature on online knapsack with deadlines and perishable-good scheduling — consult surveys of online scheduling (specific citations not recorded in the census; verify before relying on any stated bound).

## Warnings

- The census record is sparse; the exact model in arXiv:2402.08549 (objective, adversary strength, whether value vanishes or just shrinks at deadlines) must be read from the paper before formalizing the generalization — do not assume.
- "Tight" here depends heavily on the discount class: arbitrary nonincreasing discount functions likely admit no bounded competitive ratio (deadline-style impossibilities); restrict to smooth/exponential families or state otherwise.
- Common dead end: porting unit-job threshold arguments to knapsack settings ignoring packing infeasibility; competitive analysis must charge against offline packings, which are NP-hard to compute but exist informationally.
- The citation provenance ("fleet:gpt6pro...") is a secondary source; verify the paper's actual results rather than trusting the census summary.

---
*`BCGT-000199` · flagged for review*

## Acceptance guidance

Any of the following, stated for an explicitly defined model:

- **Upper bound**: a deterministic (or randomized) online algorithm with proven competitive ratio α for jobs with arbitrary sizes ≤ C and discount functions from a stated class (e.g., exponential with heterogeneous rates), with α ideally constant.
- **Lower bound**: an adversarial arrival construction showing no online algorithm (possibly randomized, via Yao) achieves ratio better than β, ideally matching α.
- **Separation results**: identify heterogeneity axes (sizes vs. decay laws vs. deadlines) that alone preserve constant competitiveness and which combinations destroy it.
- **Matching upper/lower bound** (full solution): α = β.

A referee would accept a paper-quality proof with the model fully formalized (arrival model, adversary type, allowed discount class, integrality of sizes).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/4ff0795e-74f6-43b2-bc95-a6b8cefe3fc7?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000199.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000199)
