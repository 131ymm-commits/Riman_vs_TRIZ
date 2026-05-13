
---

### Файл 2: `triz_riemann_operator.py`

```python
import numpy as np
from sympy import primerange
from scipy.linalg import eigh
from scipy.stats import ks_2samp

def main():
    # ------------------------------------------------------------
    # Parameters
    # ------------------------------------------------------------
    LIMIT = 4_000_000          # number of primes up to this value
    MODULUS = 210              # totient φ(210)=48
    np.random.seed(42)         # reproducibility

    print("Generating primes...")
    primes = list(primerange(2, LIMIT))
    print(f"Number of primes: {len(primes)}")

    # Residue classes coprime to MODULUS
    residues = [r for r in range(1, MODULUS) if np.gcd(r, MODULUS) == 1]
    res_to_idx = {r: i for i, r in enumerate(residues)}
    dim = len(residues)
    print(f"Dimension H: {dim}")

    # Frequency and transition counts
    freq = np.zeros(dim)
    transitions = np.zeros((dim, dim), dtype=int)

    for i in range(len(primes)-1):
        a = primes[i] % MODULUS
        b = primes[i+1] % MODULUS
        if a in res_to_idx and b in res_to_idx:
            ia = res_to_idx[a]
            ib = res_to_idx[b]
            freq[ia] += 1
            transitions[ia, ib] += 1

    # Deterministic part: diagonal = -log(freq), off‑diagonal = -log(probability)
    H_det = np.diag(-np.log(freq / freq.sum() + 1e-12))
    offdiag_scale = 1.10

    for i in range(dim):
        row_sum = transitions[i].sum()
        if row_sum < 20:
            continue
        for j in range(dim):
            if transitions[i, j] > 12:
                prob = transitions[i, j] / row_sum
                H_det[i, j] = -np.log(prob + 1e-9) * offdiag_scale

    H_det = (H_det + H_det.T) / 2

    # ------------------------------------------------------------
    # TRIZ step: Transition to complex phases (random unitary)
    # ------------------------------------------------------------
    H = H_det.astype(complex)
    phases = np.random.uniform(0, 2*np.pi, (dim, dim))

    for i in range(dim):
        for j in range(i+1, dim):
            amp = H_det[i, j]
            H[i, j] = amp * np.exp(1j * phases[i, j])
            H[j, i] = amp * np.exp(-1j * phases[i, j])

    # Ensure positive spectrum (shift diagonal)
    eigvals_det = np.linalg.eigvalsh(H_det)
    shift = -np.min(eigvals_det) + 0.5
    H += np.eye(dim, dtype=complex) * shift

    # Global scaling
    H = H / np.max(np.abs(H)) * 6.5

    eigvals = np.linalg.eigvalsh(H)
    eigvals = np.sort(eigvals)
    print(f"Eigenvalues: min={eigvals.min():.4f}, max={eigvals.max():.4f}")

    # ------------------------------------------------------------
    # First 70 Riemann zeta zeros (imaginary parts)
    # ------------------------------------------------------------
    riemann_zeros = np.array([
        14.1347, 21.0220, 25.0109, 30.4249, 32.9351, 37.5862, 40.9187, 43.3271,
        48.0052, 49.7738, 52.9703, 56.4462, 59.3470, 60.8318, 65.1125, 67.0798,
        69.5464, 72.0672, 75.7047, 77.1448, 79.3374, 82.9104, 84.7355, 87.4253,
        88.8091, 92.4919, 94.6513, 95.8706, 98.8312, 101.3179, 103.7255, 105.4466,
        107.1686, 111.0295, 111.8747, 114.3202, 116.2267, 118.7908, 121.3701, 122.9468,
        124.2568, 127.5167, 129.5787, 131.0877, 133.4977, 134.7565, 138.1160, 139.7362,
        141.1237, 143.1118, 146.0006, 147.4228, 150.0535, 150.9253, 153.0247, 156.1129,
        157.5976, 158.8495, 161.1882, 163.0307, 165.5371, 167.1844, 169.0945, 169.9116,
        173.4115, 174.7542, 176.4414, 178.3774, 180.8096, 182.8494
    ])

    # Unfolding (mean spacing = 1)
    def unfold(seq):
        seq = np.sort(seq)
        mean_spacing = np.mean(np.diff(seq))
        return seq / mean_spacing

    u_eig = unfold(eigvals)
    u_zeta = unfold(riemann_zeros)

    spacings_H = np.diff(u_eig)
    spacings_zeta = np.diff(u_zeta)

    ks_stat, p_val = ks_2samp(spacings_H, spacings_zeta)

    # ------------------------------------------------------------
    # Output table
    # ------------------------------------------------------------
    print("\n" + "="*40)
    print("RESULTS (Complex Hermitian, dim=48)")
    print("="*40)
    print(f"Dimension H               = {dim}")
    print(f"min eigenvalue            = {eigvals.min():.4f}")
    print(f"max eigenvalue            = {eigvals.max():.4f}")
    print(f"KS-statistic              = {ks_stat:.6f}")
    print(f"p-value                   = {p_val:.6f}")
    if p_val > 0.05:
        print("CONCLUSION: Distributions are NOT distinguishable (p > 0.05) – SUCCESS")
    else:
        print("CONCLUSION: Distributions differ (p < 0.05) – need further improvement")

if __name__ == "__main__":
    main()
