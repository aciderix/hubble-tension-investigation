# Hubble Tension — Independent Investigation

This repository documents an end-to-end, single-evening investigation of the **Hubble tension** (the ~5σ disagreement between local distance-ladder measurements of H₀ ≈ 73 km/s/Mpc and CMB/BAO inferences of H₀ ≈ 67 km/s/Mpc), carried out from scratch on public data.

> **Author note.** This work was produced collaboratively in one evening (April 28, 2026, Nantes, France) using public datasets and standard tools. It is **not original physics research** in the sense of new theoretical results — every mechanism we test (local void, evolving w(z), calibration bias, Early Dark Energy) has been explored extensively in the peer-reviewed literature. What is original here is the **independent reproduction** of these tests on a single homogeneous pipeline, the **side-by-side falsification** of three of them with the same data, and the **quantitative confirmation** that Early Dark Energy is the only one that holds up under the joint Pantheon+ + DESI BAO + CMB-prior constraint.

---

## TL;DR results

| # | Mechanism | H₀ inferred | Tension vs SH0ES (73.04) | Verdict |
|---|---|---|---|---|
| 1 | **Local void (KBC bubble)** — LTB parametric fit on Pantheon+ low-z + DESI BAO | 63.3 ± 0.4 (non-physical δ > 0) | worse | ❌ falsified by our own data: H₀(z) trend has the **wrong sign** for a local void |
| 2 | **Evolving w(z)** — w₀w_aCDM with Planck Ωm prior | 62.3 ± 0.5 (w₀ = -0.39, wₐ = -2.75) | 9.5σ | ❌ confirms the DESI 2024 w(z) ≠ -1 signal but does **not** help H₀ |
| 3 | **SH0ES calibration bias** — host-mass step + redshift-bin consistency | mass-step ΔH₀ = 0.08 ± 0.5 | n/a | ❌ no significant internal inconsistency in the distance ladder |
| 4 | **Early Dark Energy (EDE)** — f_EDE shortens the sound horizon r_d at recombination | **71.1 ± 0.2** with f_EDE = 6.8% | **1.8σ** | ✅ quantitatively resolves the tension; needs f_EDE ≈ 9.8% to hit 73.04 exactly |

The full numerical outputs, with σ estimates and Δχ² comparisons, are reproduced in [`results/RESULTS.md`](results/RESULTS.md).

---

## What is new here vs. the literature

**To be very clear about novelty:**

- ❌ **Not new:** the existence of the Hubble tension itself, the KBC void hypothesis (Keenan, Barger, Cowie 2013; Haslbauer+ 2020), the w₀wₐ DESI signal (DESI Collaboration 2024), the Early Dark Energy mechanism (Karwal & Kamionkowski 2016; Poulin et al. 2018; Smith et al. 2020), or the value of f_EDE ≈ 10% needed to fully bridge the tension.
- ❌ **Not new:** any of the data products. We use Pantheon+SH0ES (Scolnic et al. 2022, Brout et al. 2022) and DESI BAO DR1 (Adame et al. 2024).
- ✅ **What this repo adds (modest claims):**
  1. A **single self-contained pipeline** that fits all four candidate mechanisms on the *same* underlying data, allowing a direct apples-to-apples comparison in one place.
  2. An **explicit falsification of the local-void interpretation** of our own anisotropy detection — by fitting H₀(z) in low-z bins and showing the trend has the *opposite* sign to what a KBC-style underdensity would produce. This sign-test is rarely framed this directly in the public literature.
  3. **Replication, in roughly two hours of compute on a laptop-equivalent**, of the central numerical claim of the EDE programme: that an ~7–10 % early-dark-energy fraction at recombination raises the inferred H₀ from ~67 to ~71–73 while keeping the well-measured combination H₀·r_d fixed.

In short: **no new physics, no new data, but a transparent and reproducible end-to-end audit** that lets a non-specialist see exactly which solutions survive and which don't, and *why*.

---

## Caveats and known limitations

This is a one-evening exploratory pipeline. It is **not** a peer-review-grade analysis. Specifically:

- We use **only the diagonal of the Pantheon+ covariance matrix** (`MU_SH0ES_ERR_DIAG`). The full covariance includes correlated systematic terms (calibration, intrinsic scatter, peculiar velocities, photometric zero-points). Ignoring off-diagonal terms **underestimates parameter errors** by a factor of order unity. As a consequence, the multi-σ "preference" numbers reported here (e.g. 21σ for w(z) ≠ -1, 21σ for f_EDE > 0) are **inflated**; the realistic significances in the literature are typically 2–4σ.
- The EDE model is **paramaterically simplified**: we model EDE only as a multiplicative shortening of r_d, `r_d = r_d^ΛCDM × (1 - α·f_EDE)` with α = 0.85. The full physics requires solving for an axion-like scalar field with a `(1-cos φ)^n` potential (Poulin+ 2018), which we did not implement. Our `f_EDE` is therefore an *effective* parameter that captures the leading effect on r_d but not the subdominant effects on the CMB power spectrum or the matter power spectrum.
- We do **not** include the well-known **S₈ tension** that EDE introduces in large-scale-structure data. Full EDE analyses must address this; ours doesn't.
- The DESI BAO data are entered as 7 effective `D_V/r_d` data points with diagonal errors. The actual DESI likelihood has correlations between `D_M/r_d` and `D_H/r_d` per redshift bin which we omit.
- The `low-z anisotropy` result (4.5σ in z<0.03) reported during the analysis was based on a sky-direction split using particular-velocity-uncorrected redshifts. A proper analysis would correct for peculiar velocities first; the residual amplitude is likely smaller than reported.
- All optimizations are simple Nelder-Mead from multiple starts. They are **not** rigorous MCMC; reported `σ` values come from numerical Hessians at the best-fit and underestimate true uncertainties.

If anything in this README is presented with too much confidence, please read it as **"this is what we found in one evening with simplified tools, and here's how it compares to the published literature."**

---

## Repository layout

```
.
├── README.md                          ← you are here
├── LICENSE
├── scripts/
│   ├── 02_anisotropy_test.py          ← directional H₀ map, dipole detection
│   ├── 04_w0wa_DESI.py                ← w₀wₐCDM joint fit (Test B)
│   ├── 05_calibration_test.py         ← H₀ vs z bin + host-mass step (Test C)
│   └── 06_early_dark_energy.py        ← EDE r_d-shortening fit (Test A — the one that works)
├── results/
│   ├── RESULTS.md                     ← consolidated numerical outputs
│   └── summary_table.csv              ← machine-readable summary
└── docs/
    └── physics_intuition.md           ← short pedagogical note on why EDE works
```

---

## How to reproduce

Requirements: Python ≥ 3.9, `numpy`, `scipy`. No special cosmology library is used (everything is implemented from scratch in standard scipy).

```bash
pip install numpy scipy
python scripts/02_anisotropy_test.py        # ~2 minutes
python scripts/04_w0wa_DESI.py              # ~3 minutes
python scripts/05_calibration_test.py       # ~1 minute
python scripts/06_early_dark_energy.py      # ~2 minutes
```

Each script downloads Pantheon+ on the fly (no manual data fetch needed).

---

## Data sources

- **Pantheon+SH0ES** — Scolnic et al. 2022, Brout et al. 2022. Downloaded from the official release at `github.com/PantheonPlusSH0ES/DataRelease`. We use the file `Pantheon+SH0ES.dat` (1701 SNe Ia, of which 1578 non-calibrators after our quality cut).
- **DESI BAO 2024 (DR1)** — Adame et al. 2024 (arXiv:2404.03002). The 7 `D_V/r_d` data points are hardcoded in our scripts (Table 1 of the paper).
- **CMB priors (Planck 2018)** — `Ω_m = 0.315 ± 0.007`, `H₀·r_d = 9912.5 ± 30 km/s` from Aghanim et al. 2020.

---

## License

MIT. Use freely. No warranty — this is exploratory code, not production cosmology.
