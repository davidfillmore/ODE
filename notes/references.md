# References

## Textbooks

### Hairer, Norsett & Wanner (1993) — ODEs I
- **Title:** Solving Ordinary Differential Equations I: Nonstiff Problems
- **Authors:** Ernst Hairer, Syvert Norsett, Gerhard Wanner
- **Publisher:** Springer Verlag Series in Comput. Math., Vol. 8
- **DOI:** 10.1007/978-3-540-78862-1
- **BibTeX key:** `ODEsI`

### Hairer & Wanner (1996) — ODEs II
- **Title:** Solving Ordinary Differential Equations II: Stiff and Differential-Algebraic Problems
- **Authors:** Ernst Hairer, Gerhard Wanner
- **Publisher:** Springer Verlag Series in Comput. Math., Vol. 14
- **DOI:** 10.1007/978-3-662-09947-6
- **BibTeX key:** `ODEsII`
- **Note:** Foundational reference for Rosenbrock DAE methods; source of the 6-stage DAE Rosenbrock parameters used in MICM.

## Benchmark Papers

### Sandu et al. (1997) — Benchmark I: Implicit vs Explicit
- **Title:** Benchmarking Stiff ODE Solvers for Atmospheric Chemistry Problems I: Implicit vs Explicit
- **Authors:** A. Sandu, J.G. Verwer, M. Van Loon, G.R. Carmichael, F.A. Potra, D. Dabdub, J.H. Seinfeld
- **Journal:** Atmospheric Environment, 31(19), 3151–3166
- **DOI:** 10.1016/S1352-2310(97)00059-9
- **BibTeX key:** `SanduI`
- **Key findings:** Compared 5 explicit and 4 implicit solvers on 7 benchmark problems. Sparse RODAS (Rosenbrock) performed best in the 1% error region. TWOSTEP was best explicit solver for gas-phase problems.

### Sandu et al. (1997) — Benchmark II: Rosenbrock Solvers
- **Title:** Benchmarking Stiff ODE Solvers for Atmospheric Chemistry Problems II: Rosenbrock Solvers
- **Authors:** A. Sandu, J.G. Verwer, J.G. Blom, E.J. Spee, G.R. Carmichael, F.A. Potra
- **Journal:** Atmospheric Environment, 31(20), 3459–3472
- **DOI:** 10.1016/S1352-2310(97)83212-8
- **BibTeX key:** `SanduII`
- **Key findings:** Tested 8 Rosenbrock-type solvers (rodas, Ros4, rodas3, ros3, etc.). rodas3 and ros3 specially developed for air quality applications. Emphasizes sparse matrix techniques.

## Other References

### Butcher (1972) — Algebraic Theory of Integration Methods
- **Title:** An Algebraic Theory of Integration Methods
- **Author:** J.C. Butcher
- **Journal:** Mathematics of Computation, 26(117), 79–106
- **URL:** http://www.jstor.org/stable/2004720
- **BibTeX key:** `4d1669a5-f006-3a25-bbb2-7621f96df836`

### Blanes et al. (2009) — Magnus Expansion
- **Title:** The Magnus Expansion and Some of Its Applications
- **Authors:** S. Blanes, F. Casas, J.A. Oteo, J. Ros
- **Journal:** Physics Reports, 470(5), 151–238
- **DOI:** 10.1016/j.physrep.2008.11.001

### Bacsi & Kocsis (2023) — Exact Quadratic DE Solutions
- **Title:** Exactly Solvable Quadratic Differential Equation Systems Through Generalized Inversion
- **Authors:** Adam Bacsi, Albert Kocsis
- **Journal:** Qualitative Theory of Dynamical Systems, 22
- **DOI:** 10.1007/s12346-023-00738-7
- **BibTeX key:** `QDE`
