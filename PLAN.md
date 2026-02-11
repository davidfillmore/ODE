# PLAN: MICM Rosenbrock DAE Solver Paper

## Working Title

**A Rosenbrock Differential-Algebraic Solver in MICM: Architecture, Numerical Properties, and Atmospheric Chemistry Benchmarks**

## Objective

Document and validate MICM's Rosenbrock DAE implementation for index-1 chemistry constraints, with emphasis on:
- algorithm design choices in MICM,
- numerical behavior on stiff systems,
- reproducible benchmark evidence.

## LaTeX Paper Structure (Draft)

1. **Introduction**
   - Why DAE structure arises in atmospheric chemistry.
   - Why Rosenbrock methods are a practical choice for stiff chemistry.
   - Specific contribution of MICM implementation.

2. **Background and Related Work**
   - Rosenbrock/W-method history and stiff integration context.
   - DAE (index-1) formulation and row-replacement approaches.
   - Atmospheric chemistry solver benchmark lineage (Sandu I/II).

3. **MICM DAE Formulation**
   - Governing equations: `M y' = F(y)` with diagonal mass matrix.
   - Constraint residual form (`EquilibriumConstraint`).
   - Mapping from chemistry processes + constraints into solver rows.

4. **Implementation Details in MICM**
   - Build-time wiring (`SolverBuilder`, sparsity merge, algebraic row pruning).
   - Runtime assembly (forcing, Jacobian, stage solves, adaptive stepping).
   - State reordering, vectorized/scalar paths, and clamping policy.
   - Supported parameter sets (3-stage, 4-stage DAE, 6-stage DAE).

5. **Verification and Test Evidence**
   - Existing unit/integration tests (constraint residuals, conservation, reorder behavior).
   - Stress and robustness cases (stiff coupling, overlapping Jacobian deps).
   - Gaps and limitations revealed by current tests.

6. **Benchmark Design and Results**
   - Benchmark protocol (error norm, tolerances, cost metrics, hardware/compiler).
   - Atmospheric stiff benchmark subset (Sandu lineage).
   - Additional DAE-focused cases and sensitivity runs.
   - Accuracy/cost plots and solver-state diagnostics.

7. **Discussion**
   - Where MICM's architecture is strong.
   - Current limitations (constraint types, static `K_eq`, index assumptions).
   - Implications for production atmospheric chemistry workflows.

8. **Conclusions and Future Work**
   - Key findings.
   - Prioritized extensions (temperature-dependent constraints, new constraint classes, richer benchmarks).

9. **Appendices**
   - Coefficient tables actually used in MICM runs.
   - Reproducibility manifest (MICM version DOI, compile flags, machine info).
   - Supplemental derivations and pseudocode.

## Figures and Tables to Prepare

### Figures
1. Architecture flow diagram: build-time and per-step runtime pipeline.
2. Sparsity pattern illustration before/after algebraic row pruning.
3. Error-vs-cost plots for Rosenbrock parameter sets.
4. Step-size and solver-state diagnostics on a stiff DAE case.

### Tables
1. Rosenbrock parameter sets used (stages/order/source).
2. Test coverage matrix mapped to solver components.
3. Benchmark setup matrix (problem, tolerances, metrics).
4. Reproducibility metadata (version, compiler, platform).

## Source-to-Claim Map (Initial)

- **Core DAE/Rosenbrock theory:** Kaps & Rentrop (1979), Hairer-Lubich-Roche (1989), Hairer & Wanner (1996), Roche (1988).
- **Atmospheric benchmark precedent:** Sandu et al. (1997 I/II), Liao et al. (2025).
- **MICM implementation evidence:** MICM repo/docs, MICM Zenodo release records, MICM citation guidance.

## Drafting Order

1. Freeze bibliography and citation keys.
2. Write Sections 3-5 first (implementation + verification notes already available).
3. Build benchmark harness and generate Section 6 figures/tables.
4. Draft Sections 1-2 from finalized technical content.
5. Close with Discussion/Conclusions and reproducibility appendix.

## Immediate Next Actions

1. Add `paper/` directory with `main.tex`, `sections/*.tex`, and `refs.bib`.
2. Convert `notes/references.md` entries into BibTeX with stable citation keys.
3. Pin target MICM release/commit and record exact solver parameter values used in experiments.
