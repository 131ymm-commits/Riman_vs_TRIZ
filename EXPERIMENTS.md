# Detailed experiment log

All experiments performed in Google Colab (free tier).  
**Common parameters:** random seed=42, offdiag_scale=1.10, diagonal shift auto, global scale=6.5.

## Experiment series 1 – Real Hermitian operator (no phases)

| Modulus | Dim | Limit | #primes | GUE p‑value | Z(β) error | Comment |
|---------|-----|-------|---------|-------------|------------|---------|
| 30 | 8 | 4M | 283k | 0.000011 | >1000% | GUE fails completely |
| 210 | 48 | 8M | 540k | 0.000000 | 6.5% | Excellent Z(β), terrible GUE |
| 2310 | 480 | 2M | 149k | 0.0017 | **17.9%** | Best Z(β) ever |
| 2310 | 480 | 12M | 788k | 0.0770 | 95.5% | GUE borderline, Z(β) worse |

**Conclusion:** Without phases, the operator can approximate ζ(s) well (error as low as 18%), but its eigenvalue spacing distribution is far from GUE.

## Experiment series 2 – Random unitary phases

| Modulus | Dim | Limit | #primes | GUE p‑value | Z(β) error | Comment |
|---------|-----|-------|---------|-------------|------------|---------|
| 30 | 8 | 4M | 283k | **0.8426** | 283% | Perfect GUE, poor Z(β) |
| 210 | 48 | 8M | 540k | 0.7479 | 283% | Same behaviour |
| 2310 | 480 | 2M | 149k | 0.0017 | 17.9% | Not enough data for GUE |
| 2310 | 480 | 12M | 788k | 0.0770 | 95.5% | GUE borderline |

**Conclusion:** Random phases force the spectrum into GUE universality, but destroy the partition function match.

## Experiment series 3 – Deterministic phases (residue difference, mean gap)

Both completely failed: p‑value < 1e-5 for all tested deterministic phase formulas.

## Experiment series 4 – Hybrid operator (α * random phase + (1-α) * real)

Tested for dim=8 and dim=48, α from 0 to 1.

| α | dim=8, p‑value | dim=8, Z error | dim=48, p‑value | dim=48, Z error |
|---|----------------|----------------|-----------------|-----------------|
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

No α gives both p>0.05 and low Z error (error explodes when α > 0.3).

## Experiment series 5 – Gap‑correlation model

Used 50 gap bands, random phases.  
p‑value = 0.3546 (GUE achieved), Z error = 79% (still high).

## Summary of failures

- Deterministic phases (residue, gap) – complete failure (p < 1e-5).
- Hybrid with α – no trade‑off region.
- Increasing dimension to 480 without increasing primes enough – GUE fails.
- Increasing primes to 12M for 480 – GUE borderline, Z error worsens.

## Summary of successes

- Random phases consistently give GUE statistics (p>0.05) for dim=8,48 and also for gap‑correlation model.
- Real operator without phases can give Z(β) error as low as 18% (dim=480, 2M primes).
- The gap‑correlation model (50 states) achieves GUE with moderate Z error (79%).

## Final conclusion

There exists a **Heisenberg‑like uncertainty** in this family of operators:  
> *It is impossible to have both excellent GUE statistics (p>0.05) and low Z(β) error (<30%) at the same time under the current construction.*

This is an original discovery worth publishing as a research note.
