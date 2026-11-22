# EUA term structure (parametric)

Group project in Financial Optimization (Finansiell optimering),
Linköping University, IEI, autumn 2026. Group 7.

## Introduction
The EU Emissions Trading System (EU ETS) is a cap-and-trade system where
each EU Allowance (EUA) gives the right to emit one tonne of CO₂
equivalent. EUAs are traded both spot and as futures, with the futures
market on ICE being the most liquid. Futures with different maturities
together contain information about how the market prices carbon
over time.

That information is not directly observable. A futures price depends on
the spot price, the interest rate (financing cost) and the convenience
yield (the net benefit of holding the allowance rather than a futures
contract). To use the market for pricing, risk management or analysis,
these quantities must be measured as continuous curves over maturity
from a small number of noisy market prices. This is an inverse problem.

This project measures both curves with a parametric approach: a cubic
spline that is as smooth as possible while pricing the market instruments
accurately. The work builds on the inverse-problem framework from the
course lectures and on research on interest rate curve estimation.

## Purpose
Estimate the term structures of interest rates and convenience yield from
EUA futures on ICE, using a cubic spline that minimizes the integral of the
squared second derivative, and evaluate its ability to price the futures.

## Background and model
- **Futures pricing (cost of carry):**
  `F(t,T) = S_t * exp( ∫_t^T (f_r(s) - f_y(s)) ds )`,
  where `f_r` is the forward interest rate and `f_y` the convenience yield
  as a forward curve.
- **Identification:** futures prices only identify `f_r - f_y`. The rate
  curve `f_r` is estimated from separate EUR rate instruments (€STR OIS
  and/or Euribor swaps); `f_y` is then estimated from EUA futures.
- **Inverse problem** (F4–F5; Blomvall, 2017, EJOR):
  `min h(ξ) + ½ zᵀEz  s.t.  P(ξ) + Fz = P̄,  ξ ∈ Ξ`,
  with `h = ∫ f''(τ)² dτ`, a quadratic form in the spline coefficients.
- **Parametric:** a cubic spline with a limited number of knots. Group 6
  solves the non-parametric version (daily discretization), which is a
  natural benchmark.
- **Solvers:** if the spline is on `f` and errors are measured in `ln F`,
  the convenience yield step is a convex QP (`quadprog`). Swap pricing is
  nonlinear in `f` (`fmincon`).

## Repository structure
| Folder     | Content                                   | Stored in    |
|------------|-------------------------------------------|--------------|
| `src/`     | MATLAB code                               | Git          |
| `tests/`   | Tests of spline and pricing               | Git          |
| `main.m`   | Reproduces all results and figures        | Git          |
| `data/`    | Raw market data (Refinitiv/Bloomberg)     | Google Drive |
| `docs/`    | Course material (lectures, guides)        | Google Drive |
| `results/` | Generated figures and output              | Local only   |

Large data files and course PDFs are kept in the group's shared Google
Drive folder, not in Git. This keeps the repo small and avoids publishing
licensed data and course material. `data/`, `docs/` and `results/` are
gitignored.

Use these file names in `docs/` so that references are the same for
everyone:

```
docs/
  F1-introduktion-ML.pdf
  F2-stokastisk-programmering.pdf
  F3-CVaR-SP-losare.pdf
  F4-derivatprissattning-inversa-problem.pdf
  F5-optimering-i-finans.pdf
  exjobbsguide.pdf
  problembeskrivning.pdf
  projektval.pdf
```

## Getting started
1. Clone the repo:
   `git clone https://github.com/ingemarhelander/eua-term-structure.git`
2. Download the contents of the shared Google Drive folder
   (*link: to be added*) into `data/` and `docs/` in your local repo.
3. Open the repo folder in MATLAB and run `main.m`.

## Data
*To be documented:* sources, contracts, period, spot proxy, rate
instruments and cleaning rules. Each dataset in Google Drive should have
a short note on where and when it was downloaded.

## Workflow
- One branch per task, merged via pull request.
- One owner per file to avoid merge conflicts in `.m` files.
- `git pull` before starting work.
- New or updated data goes to Google Drive, never to Git.

## Open questions (for supervision 1)
- Spline on forward rate `f`, spot rate `r`, or discount factor `d`?
- Estimate rate curve and convenience yield jointly or in two steps?
- Which rate instruments (OIS vs Euribor) and which time period?
- Compare against group 6's non-parametric method?

## Report
Written in LaTeX on Overleaf (separate from this repo), following
Blomvall's thesis guide (exjobbsguiden).

| Deliverable | When | Content |
|---|---|---|
| Plan | Supervision 1 | Background, purpose, method, key references, time plan |
| Agenda | 24 h before each supervision (3 total) | Status, questions per component |
| Report 1 | Seminar 1 | Introduction, method, theory, replicable method, hours table |
| Report 2 | Seminar 2 | Report 1 + results, discussion, summary |

## Group
Filip Anderson, Juliana Eidhammer, Isak Ekbom, Ingemar Helander,
Sebastian Karis, Henrik Olsson, Thea Söilen.
