# Chapman Mechanism

Source: `slides/chapman.tex`, `slides/chapman_ode.tex`, `slides/chapman_3d_2d.tex`

## Reactions

| # | Reaction | Type |
|---|----------|------|
| 1 | O₂ + γ → O + O | Photolysis |
| 2 | O + O₂ + M → O₃ + M | Arrhenius (ozone creation) |
| 3 | O₃ + γ → O + O₂ | Photolysis (Chappuis branch) |
| 4 | O₃ + γ → O* + O₂ | Photolysis (Hartley branch) |
| 5 | O* + M → O + M | Quenching |
| 6 | O + O₃ → O₂ + O₂ | Arrhenius (ozone destruction) |

Species: O₂, O, O₃, O* (excited), M (air), γ (UV photon)

## ODE System

$$\frac{d[\text{O}]}{dt} = 2j_2[\text{O}_2] - k_2[\text{O}][\text{O}_2] + j_3[\text{O}_3] - k_3[\text{O}][\text{O}_3]$$

$$\frac{d[\text{O}_2]}{dt} = -j_2[\text{O}_2] - k_2[\text{O}][\text{O}_2] + j_3[\text{O}_3] + 2k_3[\text{O}][\text{O}_3]$$

$$\frac{d[\text{O}_3]}{dt} = k_2[\text{O}][\text{O}_2] - j_3[\text{O}_3] - k_3[\text{O}][\text{O}_3]$$

## Rate Constants

| Constant | Value | Units |
|----------|-------|-------|
| j₂ | 10⁻¹⁰ | s⁻¹ |
| k₂ | 10⁻¹⁶ | cm³ molecule⁻¹ s⁻¹ |
| j₃ | 10⁻³ | s⁻¹ |
| k₃ | 10⁻¹⁵ | cm³ molecule⁻¹ s⁻¹ |

## Dimensionless Formulation

Scale: $x_n = [\text{O}_n] / \langle[\text{O}_3]\rangle$ with $\langle[\text{O}_3]\rangle = 10^{12}$ molecules cm⁻³.

Typical values: $x_1 \sim 10^{-5}$, $x_2 \sim 2 \times 10^4$, $x_3 \sim 1$.

The system exhibits stiffness due to widely varying timescales (rates span 10⁻¹⁰ to 10⁻³ s⁻¹).

## 2D Reduced System (x₂ held constant)

$$\frac{dx_1}{dt} = 2j_2 x_2 + (-k_2 x_2) x_1 + j_3 x_3 - k_3 x_1 x_3$$

$$\frac{dx_3}{dt} = k_2 x_2 x_1 - j_3 x_3 - k_3 x_1 x_3$$

Conserved quantity: $d(x_1 + x_3)/dt = 2j_2 x_2 - 2k_3 x_1 x_3$

## MICM JSON Configuration

Photolysis reactions specify type `"PHOTOLYSIS"` with reactants and products (rates vary with time, not in config).

Arrhenius reactions specify type `"ARRHENIUS"` with rate constant `A`:
- Ozone creation: A = 1e-16
- Ozone destruction: A = 1e-15

## Python Interface

```python
import musica

solver = musica.create_solver('configs/chapman',
    musica.micmsolver.rosenbrock, num_grid_cells)

n = 1e17  # molecules cm-3, lower stratosphere
concentrations = [1e-10 * n, 0.21 * n, 1e-5 * n]
rate_constants = {
    'PHOTO.molecular oxygen photolysis': 1e-10,
    'PHOTO.ozone photolysis': 1e-3}

musica.micm_solve(solver, time_step,
    temperature, pressure, air_density,
    concentrations, rate_constants)
```
