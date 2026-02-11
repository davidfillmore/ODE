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
