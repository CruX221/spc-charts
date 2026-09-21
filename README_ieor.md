# Wafer-fab Operating Curve — IEOR Portfolio Project

An IEOR study of cycle-time, throughput, WIP, and variability in a multi-tool
semiconductor production line. Companion project to the SPC notebook —
together they cover the two halves of fab operations: quality (SPC) and flow
(IEOR / Factory Physics).

## What's in here

| File | Purpose |
|---|---|
| `fab_operating_curve.ipynb` | Main deliverable — full IEOR notebook with sim + analytical models |
| `fab_executive_summary.png` | One-page chart for the cover letter |
| `README.md` | This file |

## What it does

Builds and analyses a 4-station SimPy fab line (lithography → etch → diffusion →
CMP) using two complementary methods:

1. **Discrete-event simulation** — log-normal processing times, parallel tools
   per station, Poisson lot arrivals, full WIP tracking
2. **G/G/c analytical approximation** — Sakasegawa's formula with Whitt's
   departure-variance linking for serial queueing networks

Both methods are run side-by-side at every experiment so the simulation
validates the closed-form approximation.

## Experiments

1. **Little's Law verification** — measured WIP matches λ·W within 2%
2. **Operating-curve sweep** — cycle time vs bottleneck utilization, showing the
   hockey-stick blow-up as utilization → 100%
3. **Variability impact (VUT equation)** — cycle time tripled at constant 80%
   utilization when effective CV is raised from 0.3 to 1.6
4. **Per-station decomposition** — where in the line is the cycle time accumulating
5. **Bottleneck-shift experiment** — adding capacity at the bottleneck moves the
   entire operating curve and migrates the constraint elsewhere

## Concepts demonstrated

- Little's Law (L = λW)
- Kingman's VUT formula and Sakasegawa's G/G/c extension
- Whitt's departure-SCV linking for serial queueing networks
- Hopp & Spearman "best-case / worst-case / practical worst-case" curves
- Goldratt's Theory of Constraints (bottleneck dominance, shift)
- 85%-utilization rule of thumb for fab loading

## Reproducing

```bash
pip install numpy pandas matplotlib scipy simpy nbformat jupyter
jupyter notebook fab_operating_curve.ipynb
```

Seeds are fixed (42, 7, 1) — runs are deterministic.

## Honest limitations

- **Single-pass, not re-entrant** — real fabs visit each tool 20–40× for different
  mask layers. Re-entrant queueing networks (Kumar, Harrison) need a different
  fluid-model framework. Mentioned in §9 of the notebook.
- **No explicit MTTF/MTTR** — equipment failures are folded into the effective
  $c_e$, which is the Hopp & Spearman convention but hides preventable vs
  unpreventable variability.
- **No batch tools** — diffusion is modeled as a single-server tool. Real
  diffusion furnaces are batch (e.g., 150 wafers) and need batch-arrival
  queueing models.
- **No sequence-dependent setups** — reticle changes in litho aren't modeled.

## References

- Hopp & Spearman, *Factory Physics* (3rd ed., 2008) — chapters 8, 9, 12
- Sakasegawa (1977), *Ann. Inst. Stat. Math.* — the G/G/c approximation
- Whitt (1983), *Bell System Tech. J.* — Queueing Network Analyzer
- Kumar (1993), *Queueing Systems* — re-entrant lines
