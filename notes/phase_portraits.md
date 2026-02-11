# Phase Portraits and Dynamical Systems

Source: `slides/linear_nonlinear.tex`

## System Classifications

### Nonlinear
$$\frac{dx}{dt} = F(x, t)$$

- x is a point in state space M with dimension N
- F(x,t) is a (time-dependent) vector field on M
- x(x₀, t) represents trajectories with initial condition x(x₀, t₀) = x₀

### Linear
$$\frac{dx_i}{dt} = s_i(t) + \sum_{j=1}^N L_{ij}(t) \, x_j$$

### Quadratic
$$\frac{dx_i}{dt} = s_i(t) + \sum_{j=1}^N L_{ij}(t) \, x_j + \frac{1}{2} \sum_{j=1}^N \sum_{k=1}^N Q_{ijk}(t) \, x_j x_k$$

with symmetry $Q_{ikj} = Q_{ijk}$.

## Connection to Chapman Chemistry

The Chapman mechanism in dimensionless form decomposes into:
- **Linear part:** 3x3 matrix with photolysis rates j₂, j₃
- **Quadratic part:** x₁x₂ terms (ozone creation) and x₁x₃ terms (ozone destruction)

The 2D reduced system (x₂ constant) has a conservation law: $d(x_1 + x_3)/dt = 2j_2 x_2 - 2k_3 x_1 x_3$.
