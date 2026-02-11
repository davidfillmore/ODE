# MICM Rosenbrock DAE Architecture

Source: `~/EarthSystem/MICM/docs/design/ARCHITECTURE.md`

## Mathematical Form

### Kinetics
For species state vector y: `dy/dt = f_chem(y)`

### Constraints
For m constraints: `g_k(y) = 0`, k=1..m

Currently implemented: `EquilibriumConstraint`:
`G = K_eq * product(reactants^stoich) - product(products^stoich)`

### DAE Formulation
Diagonal mass matrix: `M * dy/dt = F(y)` with:
- M_ii = 1 for ODE rows
- M_ii = 0 for algebraic rows
- F_i = f_chem,i on ODE rows
- F_i = g_i on algebraic rows

Constrained species remain part of the state vector (no extra appended unknowns).

## State and Dimensions

- `state_size_ = n_species`
- `constraint_size_ = n_constraints` (metadata only)
- `variables_` shape: `(n_cells, n_species)`
- Jacobian shape: `n_species x n_species`
- Rosenbrock temporaries are species-sized

Mass diagonal stored in `state.upper_left_identity_diagonal_`, derived from `StateParameters.mass_matrix_diagonal_`.

## Build-Time Wiring (`SolverBuilder::Build`)

1. Build kinetic `ProcessSet` and kinetic Jacobian sparsity
2. Build `ConstraintSet` (row-replacement mode) when constraints exist
3. Collect `algebraic_variable_ids` from constraints
4. Pass algebraic IDs to `ProcessSet` so kinetic forcing/Jacobian skips algebraic rows
5. Prune kinetic sparsity entries whose dependent row is algebraic
6. Merge constraint Jacobian sparsity into the global sparsity set
7. Build species-sized sparse Jacobian and linear-solver structures
8. Set Jacobian flat IDs for rates and constraints
9. Build mass-matrix diagonal (1 ODE, 0 algebraic)

Robustness fix: `ProcessSet::SetJacobianFlatIds` skips `VectorIndex(...)` lookups for algebraic dependent rows and stores placeholder IDs to keep update-loop indexing aligned.

## Forcing and Jacobian Assembly

### Kinetic contributions (`ProcessSet`)
- Forcing and Jacobian computed for ODE rows only
- Writes into algebraic rows are skipped (scalar and vectorized paths)

### Constraint contributions (`ConstraintSet`)
- Row-replacement only: forcing row assigned to constraint residual g(y)
- Jacobian row applied with solver sign convention (-dg/dy)
- Duplicate dependency columns accumulate via subtraction on pre-zeroed rows
- Each constraint targets one algebraic species row (`GetAlgebraicSpecies()`)
- Duplicate mapping to the same row is rejected

## Rosenbrock Runtime (per step)

1. Build forcing: `ProcessSet::AddForcingTerms` + `ConstraintSet::AddForcingTerms`
2. Build Jacobian: `ProcessSet::SubtractJacobianTerms` + `ConstraintSet::SubtractJacobianTerms`
3. Form/factor: `A = alpha * M - J`
4. Stage loop: `c_ij/H` coupling scaled row-wise by M_ii; algebraic rows (M_ii=0) skip stiff 1/H term
5. Candidate update + embedded error + adaptive step control

## Post-Solve Clamping

- DAE mode (`constraints_replace_state_rows_ == true`): clamp only ODE rows (M_ii > 0)
- Non-DAE mode: clamp all rows

## State Reordering

`SetReorderState(true)` is supported with constraints:
- Species map reordered by sparsity heuristics
- Algebraic row IDs resolved using reordered indices
- Mass-matrix diagonal and row replacement follow reordered mapping

## Known Limitations

1. Only `EquilibriumConstraint` type implemented
2. Equilibrium constants are static (no built-in temperature dependence)
3. Assumes index-1 algebraic constraints; no automatic index reduction
4. Algebraic-row target for equilibrium constraints is the first product species

## Extension: Adding a New Constraint Type

1. Implement `Residual(...)`, `Jacobian(...)`, `AlgebraicSpecies()`, dependency/species-name accessors
2. Add to `Constraint::ConstraintVariant`
3. Ensure dependency indexing and Jacobian sparsity rows in `ConstraintSet`
4. Add integration coverage for mixed coupling, reordered-state, vectorized/scalar paths

## Core File Map

| File | Role |
|------|------|
| `include/micm/solver/solver_builder.inl` | Build-time wiring |
| `include/micm/solver/solver.hpp` | Solver interface |
| `include/micm/solver/rosenbrock.inl` | Rosenbrock runtime |
| `include/micm/solver/state.hpp` / `.inl` | State management |
| `include/micm/solver/rosenbrock_temporary_variables.hpp` | Temporaries |
| `include/micm/process/process_set.hpp` | Kinetic rates |
| `include/micm/constraint/constraint.hpp` | Constraint variant |
| `include/micm/constraint/constraint_set.hpp` | Constraint set |
| `include/micm/constraint/equilibrium_constraint.hpp` | Equilibrium impl |
