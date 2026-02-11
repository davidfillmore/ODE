# References (Verified 2026-02-11)

## Core Rosenbrock/DAE Theory

### Kaps & Rentrop (1979) — Rosenbrock W-methods for stiff ODEs
- **Title:** Generalized Runge-Kutta methods of order four with stepsize control for stiff ordinary differential equations
- **Authors:** P. Kaps, P. Rentrop
- **Journal:** Numerische Mathematik, 33, 55-68
- **DOI:** 10.1007/BF01396495
- **Use in paper:** Historical linearly-implicit Rosenbrock foundation and adaptive control context.

### Hairer, Lubich & Roche (1989) — Runge-Kutta/DAE foundations
- **Title:** The Numerical Solution of Differential-Algebraic Systems by Runge-Kutta Methods
- **Authors:** E. Hairer, C. Lubich, M. Roche
- **Book series:** Numerical Analysis, Lecture Notes in Mathematics 1409
- **Pages:** 111-151
- **DOI:** 10.1007/BFb0093947
- **Use in paper:** Index-1 DAE background, order conditions, and stiff accuracy framing.

### Roche (1988) — Rosenbrock methods for DAEs
- **Title:** Rosenbrock Methods for Differential Algebraic Equations
- **Author:** M. Roche
- **Journal:** Numerische Mathematik, 52(1), 45-64
- **URL:** https://eudml.org/doc/133193
- **Use in paper:** Direct DAE Rosenbrock treatment relevant to MICM's mass-matrix row replacement.

### Hairer & Wanner (1996) — ODEs II (stiff + DAE reference)
- **Title:** Solving Ordinary Differential Equations II: Stiff and Differential-Algebraic Problems
- **Authors:** E. Hairer, G. Wanner
- **Series:** Springer Series in Computational Mathematics, Vol. 14
- **DOI:** 10.1007/978-3-662-09947-6
- **Use in paper:** Canonical reference for stiff DAE integrators and high-order Rosenbrock-W variants.

## Atmospheric Chemistry Solver Benchmarks

### Sandu et al. (1997) — Benchmark I
- **Title:** Benchmarking stiff ODE solvers for atmospheric chemistry problems I: Implicit versus explicit
- **Journal:** Atmospheric Environment, 31(19), 3151-3166
- **DOI:** 10.1016/S1352-2310(97)00059-9
- **Use in paper:** Comparative runtime/accuracy framing against legacy atmospheric chemistry solvers.

### Sandu et al. (1997) — Benchmark II
- **Title:** Benchmarking stiff ODE solvers for atmospheric chemistry problems II: Rosenbrock solvers
- **Journal:** Atmospheric Environment, 31(20), 3459-3472
- **DOI:** 10.1016/S1352-2310(97)83212-8
- **Use in paper:** Rosenbrock-family performance context; useful for method choice discussion.

### Liao et al. (2025) — Modern KPP Rosenbrock step-size control
- **Title:** Reducing time-step-size control errors in Rosenbrock solvers for atmospheric chemistry through improved treatment of linear systems
- **Journal:** Geoscientific Model Development, 18, 4273-4288
- **DOI:** 10.5194/gmd-18-4273-2025
- **Use in paper:** Current best-practice issues in adaptive Rosenbrock workflows (linear-system treatment).

## MICM/MUSICA Project Sources

### MICM repository
- **URL:** https://github.com/NCAR/micm
- **Use in paper:** Code-level source of truth for solver architecture, constraints, and tests.

### MICM docs and citation guidance
- **URL:** https://ncar.github.io/micm/
- **Citation page URL:** https://ncar.github.io/micm/citing_and_bibliography/index.html
- **Observed citation target (current docs):** MICM Zenodo citation DOI 10.5281/zenodo.8377912
- **Use in paper:** Official project-recommended citation and documentation baseline.

### MICM software DOI (concept + release)
- **Concept DOI (all versions):** 10.5281/zenodo.7940162
- **Current citation DOI shown in MICM docs:** 10.5281/zenodo.8377912
- **Landing page:** https://zenodo.org/records/8377912
- **Use in paper:** Reproducibility anchor for software versioning and archived artifacts.

## Implementation-Adjacent References

### PETSc TSROSWSANDU3 documentation
- **URL:** https://petsc.org/release/manualpages/TS/TSROSWSANDU3/
- **Use in paper:** External confirmation of Sandu3-family Rosenbrock coefficients in an established solver stack.

## To Verify Next (before manuscript submission)

1. Exact MICM version/commit used for all paper figures and tables.
2. Whether MICM's four-stage and six-stage DAE parameter sets map exactly to specific Hairer-Wanner tables.
3. Pull the MICM docs citation BibTeX block verbatim into `paper/refs.bib` before submission freeze.
