# DAE Solver Tests

Source: `~/EarthSystem/MICM/test/`

## Integration Tests: `test/integration/test_equilibrium.cpp` (1,157 lines)

| Test | System | Notable |
|------|--------|---------|
| ConstraintSetAPITest | A+B<->AB (K_eq=1000) | Unit-level residual check |
| SetConstraintsAPIWorks | — | Mass-matrix diagonal (1/0) |
| SetConstraintsAPIMultipleConstraints | 6 species, 2 constraints | K_eq=5, 20 |
| DAESolveWithConstraint | A->B, B<->C (K_eq=2) | Basic DAE, tol 1e-6 |
| DAESolveWithConstraintAndReorderState | A->B, B<->C | State reordering preserved |
| DAESolveWithFourStageDAEParameters | Same | 4-stage order-3 Rosenbrock |
| DAESolveWithSixStageDAEParameters | Same | 6-stage order-4 (Hairer-Wanner) |
| DAESolveWithTwoCoupledConstraints | A->B, B<->C, B<->D | K_eq=3, 5 |
| DAEConservationLaw | A+B conserved | Long integration (0.5s) |
| DAESolveStiffCoupling | K_eq=1000 | Relaxed tol 1e-4, checks NaN/Inf |
| DAESolveWithNonUnitStoichiometry | C->A, 2A<->B | Quadratic constraint |
| DAESolveMultiGridCell | 3 grid cells | Per-cell validation |
| DAEClampingDoesNotBreakAlgebraicVariables | — | Algebraic vars not clamped |
| DAEStateCopyAndSolve | — | Clone() preserves temporaries |
| DAEOverlappingSpeciesJacobian | A+B<->A+C | Duplicate dependency columns |
| DAEConstraintOnlySpeciesNotUnused | C constraint-only | Not flagged as unused species |

## Unit Tests: `test/unit/constraint/test_constraint_set.cpp` (660 lines)

- ConstraintSet construction and basic API
- Sparsity structure (`NonZeroJacobianElements`)
- Residual computation (`AddForcingTerms`)
- Jacobian row replacement (`SubtractJacobianTerms`)
- Empty constraint set (no-op behavior)
- Unknown species error handling
- 3-species / 1-constraint system (K_eq=50)
- 4-species / 2-constraint system
- Coupled constraints with shared species
- Vectorized matrix support (VectorMatrix with SparseMatrixVectorOrdering)

## Unit Tests: `test/unit/solver/test_rosenbrock.cpp`

- Normalized error calculation excludes constraint-appended columns
- Sets extreme values in constraint columns to verify they are ignored
- Tests with standard and vectorized matrix layouts (L=4)

## Unit Tests: `test/unit/solver/test_solver_builder.cpp`

- `DAEKineticAlgebraicRowPruningDoesNotThrow`: kinetics writes to algebraic row C; constraint B<->C (K_eq=2)
- `DAEKineticAlgebraicReactantRowPruningDoesNotThrow`: C is both reactant and algebraic

## Rosenbrock DAE Solver Parameters

| Method | Stages | Order | Notes |
|--------|--------|-------|-------|
| ThreeStageRosenbrockParameters | 3 | 3 | L-stable; default for most tests |
| FourStageDifferentialAlgebraicRosenbrockParameters | 4 | 3 | Stiffly-stable |
| SixStageDifferentialAlgebraicRosenbrockParameters | 6 | 4 | Hairer & Wanner |

## Validation Patterns

1. Solver convergence: `result.state_ == SolverState::Converged`
2. Constraint satisfaction: |K_eq * [reactants] - [products]| < tolerance
3. Mass conservation: sum of species conserved across time steps
4. No NaN/Inf in state or Jacobian
5. Algebraic variables derived correctly (e.g., C = K_eq * B)

## Typical Tolerances

| Context | Tolerance |
|---------|-----------|
| Constraint residual (general) | 1e-6 |
| Constraint residual (stiff, K_eq=1000) | 1e-4 |
| Jacobian unit tests | 1e-10 to 1e-12 |
| State relative tolerance | 1e-3 |

## Gaps / Not Tested

- Robertson DAE benchmark
- Temperature-dependent K_eq
- Higher-index DAE systems
- Constraint types other than EquilibriumConstraint

## Paper-Critical Benchmark Additions (Planned)

1. **Atmospheric stiff ODE benchmarks (Sandu 1997 I/II):**
   - Reproduce at least one gas-phase benchmark setup used in Atmospheric Environment 31(19/20).
   - Report error-vs-cost curves with MICM's 3/4/6-stage Rosenbrock variants.
2. **Index-1 DAE regression suite:**
   - Add canonical index-1 algebraic examples beyond equilibrium chemistry toy systems.
   - Include failure-mode checks for inconsistent initial conditions.
3. **Step-size controller stress test (Liao et al., 2025):**
   - Evaluate sensitivity of accepted step count and global error to linear-system treatment.
   - Compare current controller behavior to recommendations from KPP-focused analysis.
4. **Versioned reproducibility harness:**
   - Pin MICM release DOI and compiler settings in test metadata.
   - Emit machine-readable benchmark summaries for manuscript tables.

## Citation Links for This Test Roadmap

- Sandu et al. (1997) I: https://doi.org/10.1016/S1352-2310(97)00059-9
- Sandu et al. (1997) II: https://doi.org/10.1016/S1352-2310(97)83212-8
- Liao et al. (2025): https://doi.org/10.5194/gmd-18-4273-2025
