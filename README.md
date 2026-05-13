# Riman_vs_TRIZ – Quantum‑Evolutionary Model of the Riemann Hypothesis

**Concept author:** [131ymm-commits](https://github.com/131ymm-commits) – Evolutionary theory of transitions between levels of reality, residue‑class transition matrices, and the original operator construction.  
**TRIZ analysis & computational experiments:** DeepSeek (collaborative work).  

## Main result

A one‑parameter family of Hermitian operators built from prime number transitions reproduces **either** the GUE statistics of Riemann zeta zeros **or** the shape of ζ(1+β) via partition function, **but never both simultaneously**. This appears as a **Heisenberg‑like uncertainty principle** in the model.

| Model | Dimension | #primes | GUE p‑value | Z(β) error | Notes |
|-------|-----------|---------|-------------|------------|-------|
| Real H, mod 30 | 8 | 283k | 0.000011 | >1000% | No GUE |
| Random phases, mod 30 | 8 | 283k | 0.8426 | 283% | Excellent GUE |
| Random phases, mod 210 | 48 | 540k | 0.7479 | 283% | Stable GUE |
| Real H, mod 2310 | 480 | 149k | 0.0017 | **17.9%** | Excellent Z(β) |
| Random phases, mod 2310 | 480 | 788k | 0.0770 | 95.5% | Borderline GUE |
| Gap‑correlation model | 50 | 540k | 0.3546 | 79% | Moderate GUE |

No configuration achieved both p>0.05 and Z(β) error <30% simultaneously.

## Repository contents

- `operator_riemann_triz.py` – construction of the operator with random phases (dimension 48, 8M primes).
- `EXPERIMENTS.md` – detailed logs of all experiments (successful and failed).
- `TRIZ_ANALYSIS.md` – application of TRIZ principles and future directions.

## How to run

1. Install dependencies: `pip install numpy sympy scipy`
2. Run `python operator_riemann_triz.py` (adjust LIMIT and MODULUS inside).

## License

MIT
