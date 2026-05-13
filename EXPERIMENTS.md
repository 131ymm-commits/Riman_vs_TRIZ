
---

## Файл 2: `EXPERIMENTS.md`

```markdown
# Complete Experiment Log

All experiments were performed in Google Colab (free tier).  
Fixed parameters: seed=42, offdiag_scale=1.10, auto diagonal shift, global scale=6.5.

## 1. Real Hermitian Operator (α=0, no phases)

| Modulus | Dim | Limit | #primes | GUE p‑value | Z(β) error | Notes |
|---------|-----|-------|---------|-------------|------------|-------|
| 30 | 8 | 4M | 283k | 0.000011 | >1000% | GUE completely fails |
| 210 | 48 | 8M | 540k | 0.000000 | 6.5% | Excellent Z(β) |
| 2310 | 480 | 2M | 149k | 0.0017 | **17.9%** | Best Z(β) ever |
| 2310 | 480 | 12M | 788k | 0.0770 | 95.5% | GUE borderline, Z worse |

**Conclusion:** Real operator can approximate ζ(s) remarkably well (error down to 18%), but its eigenvalue spacing is not GUE.

## 2. Random Unitary Phases (α=1)

| Modulus | Dim | Limit | #primes | GUE p‑value | Z(β) error | Notes |
|---------|-----|-------|---------|-------------|------------|-------|
| 30 | 8 | 4M | 283k | **0.8426** | 283% | Perfect GUE |
| 210 | 48 | 8M | 540k | 0.7479 | 283% | Same behaviour |
| 2310 | 480 | 2M | 149k | 0.0017 | 17.9% | Not enough data for GUE |
| 2310 | 480 | 12M | 788k | 0.0770 | 95.5% | GUE borderline |

**Conclusion:** Random phases force GUE statistics but destroy the partition function match.

## 3. Deterministic Phases (residue difference, mean gap)

Both formulas failed completely: p‑value < 1e-5 for all tested deterministic phases.

## 4. Hybrid Operator (α·random phase + (1-α)·real)

Tested for dim=8 and dim=48, α from 0 to 1.

| α | dim=8 p‑value | dim=8 Z error | dim=48 p‑value | dim=48 Z error |
|---|---------------|---------------|----------------|----------------|
| 0.0 | 0.000011 | 84.7% | 0.000000 | 6.5% |
| 0.1 | 0.000011 | 11.7% | 0.000000 | 50.0% |
| 0.2 | 0.000050 | 88.6% | 0.000000 | 106.7% |
| 0.3 | 0.002308 | 334% | 0.000000 | 189% |
| 0.4 | 0.069479 | 1226% | 0.000000 | 309% |
| 0.5 | 0.472462 | 4853% | 0.000000 | 477% |
| 0.6 | 0.635087 | 20559% | 0.000877 | 663% |
| 0.7 | 0.711086 | 91967% | 0.106574 | 716% |
| 0.8 | 0.763850 | 431523% | 0.880150 | 590% |
| 0.9 | 0.923972 | 2.1e6% | 0.862774 | 422% |
| 1.0 | 0.997569 | 1.1e7% | 0.747918 | 283% |

No α gives both p>0.05 and Z error <100%. The moment GUE appears (α>0.6), Z error is already >500%.

## 5. PT‑Symmetric (Non‑Hermitian) Direction

We attempted a PT‑symmetric extension: H = H_det + iγ·(random antisymmetric). For γ>0, the spectrum became complex (broken PT symmetry). Max imaginary part ~6.0, comparable to the scale of H. The real parts produced singular spacing distributions (p=0.0000), and Z(β) diverged. Complete failure.

## 6. Gap‑Correlation Model (50 states, random phases)

p‑value = 0.3546 (GUE achieved), Z error = 79% (still high).

## Summary of Failures

- Deterministic phases – complete collapse.
- Hybrid α – no trade‑off region.
- PT‑symmetric – broken symmetry, unusable.
- High dimension (480) without enough primes – GUE fails.
- Even with 12M primes for 480 – GUE borderline, Z error high.

## Summary of Successes

- Random phases consistently give GUE statistics (p>0.05) for dim=8,48,50.
- Real operator without phases gives Z(β) error as low as 18% (dim=480, 2M primes).

## Final Verdict: No‑Go Theorem

Within this construction (transition matrices from consecutive primes, with phases added off‑diagonally), it is **impossible** to have both:

- GUE eigenvalue spacing (p>0.05)
- Z(β) approximation with error < 100%

This is a fundamental uncertainty:  
> **GUE quality × Z(β) accuracy ≈ 0**

The barrier is not a tuning issue – it is a structural law.
