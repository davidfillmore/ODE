# Mathematical Notation

Source: `~/DavidFillmore/ODE.wiki/`

## State and Dimensions

| Symbol | Meaning |
|--------|---------|
| N | Dimension of state space |
| M | Number of constraints |
| x_i(t), i=1..N | State variables |
| y_j(t), j=1..M | Constraint (algebraic) parameters |

## ODE Formulations

### General Nonlinear
$$\frac{dx_i}{dt} = F_i(x_j, t)$$

### Linear Systems
$$\frac{dx_i}{dt} = s_i(t) + \sum_{j=1}^N L_{ij}(t) \, x_j$$

### Quadratic Systems
$$\frac{dx_i}{dt} = s_i(t) + \sum_{j=1}^N L_{ij}(t) \, x_j + \frac{1}{2} \sum_{j=1}^N \sum_{k=1}^N Q_{ijk}(t) \, x_j x_k$$

with symmetry $Q_{ikj} = Q_{ijk}$.

## DAE Extension

- Parameters: $y_j(t)$, $j = 1, \ldots, M$
- Constraints: $G_k(x_i, y_j) = 0$, $k = 1, \ldots, M$

## Rosenbrock Method

### Conventions
- T time steps: $t_n = t_0 + n\,h$, index $n = 0, \ldots, T$
- S stages, indices (r), (s)

### Stage Sum
$$x_i(t_{n+1}) = x_i(t_n) + \sum_{r=1}^S b_r \, k_i^{(r)}$$

### Jacobian
$$J_{ij}(t_n) = \frac{\partial F_i}{\partial x_j}(t_n)$$

### Stage Equation
$$\sum_{j=1}^N \left(\delta_{ij} - h\,\gamma\,J_{ij}(t_n)\right) k_j^{(r)} = h\,F_i\!\left(x_i(t_n) + \sum_{s=1}^{r-1} a_{rs}\,k_i^{(s)},\; t_n + c_r\,h\right) + h \sum_{j=1}^N J_{ij}(t_n) \sum_{s=1}^{r-1} \gamma_{rs}\,k_j^{(s)}$$

### Runge-Kutta as Special Case
Set $\gamma = 0$ and $\gamma_{rs} = 0$:
$$k_i^{(r)} = h\,F_i\!\left(x_i(t_n) + \sum_{s=1}^{r-1} a_{rs}\,k_i^{(s)},\; t_n + c_r\,h\right)$$

### Matrix Form
$$[x]_{n+1} = [x]_n + \sum_{r=1}^S b_r\,[k]^{(r)}$$

$$(I - h\,\gamma\,[F_x]_n)\,[k]^{(r)} = h\,[F]_n^{(r-1)} + h\,[F_x]_n \sum_{s=1}^{r-1} \gamma_{rs}\,[k]^{(s)}$$

### Block Form with Constraints

$$\begin{bmatrix} x \\ y \end{bmatrix}_{n+1} = \begin{bmatrix} x \\ y \end{bmatrix}_n + \sum_{r=1}^S b_r \begin{bmatrix} k \\ l \end{bmatrix}^{(r)}$$

$$\begin{bmatrix} I - h\,\gamma\,F_x & -h\,\gamma\,F_y \\ -h\,\gamma\,G_x & -h\,\gamma\,G_y \end{bmatrix}_n \begin{bmatrix} k \\ l \end{bmatrix}^{(r)} = h \begin{bmatrix} F \\ G \end{bmatrix}_n^{(r-1)} + h \begin{bmatrix} F_x & F_y \\ G_x & G_y \end{bmatrix}_n \sum_{s=1}^{r-1} \gamma_{rs} \begin{bmatrix} k \\ l \end{bmatrix}^{(s)}$$
