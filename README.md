# EUA term structure (parametric)

Group project in Financial Optimization (Finansiell optimering),
Linköping University, IEI, autumn 2026. Group 7.

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
- **Inverse problem** (lectures 4–5; Blomvall, 2017, EJOR):
  `min h(ξ) + ½ zᵀEz  s.t.  P(ξ) + Fz = P̄,  ξ ∈ Ξ`,
  with `h = ∫ f''(τ)² dτ`, a quadratic form in the spline coefficients.
- **Parametric:** a cubic spline with a limited number of knots. Group 6
  solves the non-parametric version (daily discretization), which is a
  natural benchmark.
- **Solvers:** if the spline is on `f` and errors are measured in `ln F`,
  the convenience yield step is a convex QP (`quadprog`). Swap pricing is
  nonlinear in `f` (`fmincon`).

## Repository structure
| Folder     | Content                                   | In git |
|------------|-------------------------------------------|--------|
| `src/`     | MATLAB code                               | yes    |
| `tests/`   | Tests of spline and pricing               | yes    |
| `data/`    | Raw market data (Refinitiv/Bloomberg)     | no     |
| `results/` | Generated figures and output              | no     |
| `docs/`    | Course material (lectures, guides)        | no     |
| `main.m`   | Reproduces all results and figures        | yes    |

Raw data is not committed because of license restrictions. Each member
fetches data locally (see Data below).

## Getting started
1. Clone the repo and open it in MATLAB.

## Data
*To be documented:* sources, contracts, period, spot proxy, rate
instruments and cleaning rules.

## Workflow
- One branch per task, merged via pull request.
- One owner per file to avoid merge conflicts in `.m` files.
- `git pull` before starting work.

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