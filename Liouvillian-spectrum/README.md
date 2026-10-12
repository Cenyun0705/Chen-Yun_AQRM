# Liouvillian spectra at Ncut = 100

This directory contains spectra of the full FP-QME Liouvillian and the commutator generator of the effective Hermitian AQRM at the same fixed parameter point, $g=2.5$. Each CSV contains the first 10,000 eigenvalues with positive real part, sorted by real part. Both real and imaginary parts are retained.

## Data files

| File | Model | Exported eigenvalues |
| --- | --- | --- |
| `full-positive_ReE_first10000_complete_count.csv` | Full FP-QME Liouvillian, including the environmental terms | 10,000 |
| `eff-positive_ReE_first10000_complete_count.csv` | Liouville-space commutator generator of the effective Hermitian AQRM | 10,000 |

The prefixes `full-` and `eff-` distinguish the two originally generated CSV files. These are instantaneous spectra at one value of $g$; no branch tracking or color-based reconnection is used.

## Parameters

The Brownian-oscillator fitting step is not repeated in these calculations. The fitted parameters are supplied directly.

| Parameter | Value |
| --- | --- |
| Original friction, `zeta_input` | `1.0` |
| Two-level splitting, `Delta` | `5.0` |
| Bias, `epsilon` | `2.0 * 0.8670217872367088 = 1.7340435744734175` |
| Bare coupling, `g` | `2.5` |
| Inverse temperature, `beta` | `1.0` |
| `barOmega` | `1.000562895442171` |
| `bar_zeta` | `0.9987979339035183` |
| `delta0` | `0.08029553992562366` |
| `delta1` | `0.07681533814123918` |
| `delta2` | `-0.0006022080159356902` |
| Fock cutoff, `Ncut` | `100`, with oscillator states $n=0,\ldots,99$ |
| `INCLUDE_E0` | `True` |
| Export count | `10000` |
| Positive-sign review tolerance | `1e-9`, used only for a diagnostic flag |

The two-level system and oscillator give a Hilbert-space dimension $D=2N_{\mathrm{cut}}=200$ and a Liouville-space dimension $D^2=40000$. The cutoff refers to the oscillator basis, not to an HEOM hierarchy depth.

## Spectral convention

For the density-matrix evolution

$$
\frac{d\rho}{dt}=\mathcal L\rho,
$$

let $\lambda$ denote an eigenvalue of $\mathcal L$. The CSV uses the energy-like convention

$$
\mathcal E=i\lambda,
\qquad \lambda=-i\mathcal E.
$$

Thus `Re_E` and `Im_E` are the real and imaginary parts of $\mathcal E$, while `Re_lambda` and `Im_lambda` refer to $\lambda$. In particular,

$$
\mathrm{Re}(\mathcal E)=-\mathrm{Im}(\lambda),
\qquad
\mathrm{Im}(\mathcal E)=\mathrm{Re}(\lambda).
$$

The positive-real-part selection is applied to $\mathcal E$, **not** to the real part of $\lambda$.

### Effective model

The effective Hamiltonian is

$$
H_{\mathrm{eff}}=
\omega_{\mathrm{eff}}a^\dagger a
+g_{\mathrm{eff}}\sigma_z(a+a^\dagger)
+\frac{\Delta}{2}\sigma_x
+\frac{\epsilon}{2}\sigma_z
+E_0 I,
$$

with

$$
\omega_{\mathrm{eff}}=\sqrt{\bar\Omega^2-\bar\zeta^2/4},
\qquad
g_{\mathrm{eff}}=g(1+\delta_2/2)\sqrt{\bar\Omega/\omega_{\mathrm{eff}}},
\qquad
E_0=(\omega_{\mathrm{eff}}-\bar\Omega)/2.
$$

For this parameter set, $\omega_{\mathrm{eff}}\simeq0.8670217872367088$, $g_{\mathrm{eff}}\simeq2.6848276543989193$, and $E_0\simeq-0.06677055410273114$.

The effective Liouvillian is defined solely by

$$
\mathcal L_{\mathrm{eff}}\rho=-i[H_{\mathrm{eff}},\rho].
$$

If $E_i$ are the Hamiltonian eigenvalues, its eigenoperators $|i\rangle\langle j|$ have

$$
\lambda_{ij}=-i(E_i-E_j),
\qquad \mathcal E_{ij}=E_i-E_j.
$$

All 200 Hamiltonian eigenvalues are computed, and all 40,000 ordered differences are formed, including diagonal zeros and repeated differences. This gives the complete spectrum of the finite commutator generator without diagonalizing a dense $40000\times40000$ matrix. Its `Im_E` values are zero. The common shift $E_0$ cancels exactly from the differences.

The parameters `delta0` and `delta1` enter the full FP-QME generator but do not enter this effective Hermitian Hamiltonian. The effective dataset therefore represents the commutator spectrum, not the full dissipative dynamics.

### Full FP-QME model

All 40,000 eigenvalues of the finite full FP-QME Liouvillian are computed before selection. A real representation on the complete Hermitian operator basis is used for the numerical eigensolve; this is a change of basis, not a reduction of the Liouville-space dimension. No sparse spectral window is used.

The full spectrum is generally complex. Its real part in the $\mathcal E=i\lambda$ convention can be compared with the effective energy differences, while its imaginary part retains the modal growth or decay rate. Equal row numbers in the two sorted CSV files do not establish a correspondence between individual modes.

## Selection and numerical precision

1. Select every computed eigenvalue satisfying `Re_E > 0.0`.
2. Sort by `Re_E` in ascending order. Ties are ordered by `Im_E`, then by the index in the unfiltered spectrum.
3. Export the first 10,000 entries. Repeated eigenvalues are retained; no deduplication is performed.
4. Set `positive_sign_needs_review = 1` for a selected value with `0 < Re_E <= 1e-9`. This flag does not remove the value or set it to zero.

The reported full run has 19,902 positive-real-part entries before selection; the effective calculation has 19,900 positive differences before selection. These counts describe the computed floating-point spectra. The literal positive filter is not a mathematical certification of the sign of a numerically unresolved value.

## CSV columns

| Column | Meaning |
| --- | --- |
| `rank` | Export rank, from 1 to 10,000 |
| `Re_E` | Real part of $\mathcal E=i\lambda$ |
| `Im_E` | Imaginary part of $\mathcal E=i\lambda$ |
| `Re_lambda` | Real part of the generator eigenvalue $\lambda$ |
| `Im_lambda` | Imaginary part of $\lambda$ |
| `positive_sign_needs_review` | Diagnostic flag for a small positive real part |
| `full_spectrum_index` | Zero-based index in the unfiltered numerical spectrum |
| `level_i`, `level_j` | Effective CSV only: zero-based Hamiltonian level indices for $E_i-E_j$ |

The `full_spectrum_index` is an array index, not a symmetry label or a Hamiltonian level index. The CSV files are selected subsets; they do not contain all 40,000 eigenvalues.

## Reading the data

```python
import pandas as pd

full = pd.read_csv('full-positive_ReE_first10000_complete_count.csv')
eff = pd.read_csv('eff-positive_ReE_first10000_complete_count.csv')

E_full = full['Re_E'].values + 1j * full['Im_E'].values
E_eff = eff['Re_E'].values + 1j * eff['Im_E'].values
```

The effective dataset describes static energy differences of $H_{\mathrm{eff}}$. Agreement or disagreement with the full spectrum must be assessed without identifying the hidden symmetry of $H_{\mathrm{eff}}$ with a symmetry of the full dissipative generator.
