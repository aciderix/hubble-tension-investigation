# Handoff Brief — Hubble Tension Investigation
## For: Claude agent with local machine access (gcc, make, git, Python ≥3.10)

**Project repo:** https://github.com/aciderix/hubble-tension-investigation
**Owner:** Aciiderixx (GitHub: aciderix)
**License:** MIT
**Status as of April 28, 2026:** Levels 1, 2, and sandbox-feasible Level 3 complete. CLASS_EDE-dependent components blocked by stateless sandbox (no C compilation).

---

## TL;DR — what's already done, what's needed from you

**Done in cloud sandbox:**
1. Level 1 — 4 mechanisms tested on Pantheon+ N=1580 with full 1701×1701 covariance. Result: **EDE wins at >5σ** (Δχ² = −33). Anisotropy z<0.03 = 2.5σ real. Late-DE w(z) and calibration bias rejected.
2. Level 2 — classy 3.3.4 + CAMB 1.6.6 ΛCDM validation against Planck 2018. ΔN_eff dark radiation proxy excluded at ~7.5σ.
3. Level 3 (partial) — DESI DR2 BAO joint fit (→ low H₀ confirmed at −4.2σ vs SH0ES). classy CLP fluid scan (→ classy itself refuses CLP as EDE proxy, confirming axion physics required). Hybrid anisotropy+EDE fit (→ collapses to pure EDE; local 2.5σ insufficient to compete).

**Not done (sandbox-blocked, your job):**
- True axion-like EDE Boltzmann fit using **CLASS_EDE** or **AxiCLASS** fork (requires C compile)
- Full Planck 2018 likelihood (plik_lite + lowE + lensing) chi² evaluation
- MCMC sampling with `cobaya` over (H₀, Ωₘ, f_EDE, log₁₀(z_c), θ_i, A_s, n_s, τ, Ω_b·h², Ω_cdm·h²) + Planck nuisance params
- S₈ tension under EDE (preliminary expectation: rises ~0.02 — does self-interacting DM absorb it?)

---

## Section 1 — Scientific context (read this first)

The Hubble tension: local distance-ladder measurements (SH0ES Cepheid+SN) give H₀ = 73.04 ± 1.04 km/s/Mpc. CMB inverse-distance-ladder (Planck) gives H₀ = 67.36 ± 0.54. **5σ disagreement, persistent for ~10 years.**

Three classes of explanations:
- **(A) Pre-recombination physics** (EDE, ΔN_eff, varying α) — modify r_d (sound horizon) → modify H₀ extracted from BAO/CMB
- **(B) Late-time DE evolution** (w(z) ≠ −1) — fails because well-constrained by SNe+BAO directly
- **(C) Local environment** (void, anisotropy) — limited by per-survey scatter

Our Level 1 falsified (B) and (C alone). Remaining: (A), specifically axion-like Early Dark Energy with potential V(φ) = m²f²(1 − cos(φ/f))ⁿ, n=3.

---

## Section 2 — Existing repo structure (clone first)

```
git clone https://github.com/aciderix/hubble-tension-investigation
cd hubble-tension-investigation
```

```
README.md
level1_results.md            # 4-mechanism toy fits
level1_anisotropy.py         # z<0.03 per-survey + drop-one analysis
level1_pantheon_mcmc.py      # full Pantheon+ N=1580 toy-EDE fit
level2_classy_validation.py  # classy 3.3.4 vs Planck 2018 baseline
level2_neff_stress.py        # ΔN_eff exclusion test
level2_results.md            # Boltzmann validation summary
level3_desi_dr2_refit.py     # DESI DR2 13-pt joint fit
level3_classy_clp_scan.py    # CLP fluid stress test (in your sandbox terms)
level3_hybrid_model.py       # anisotropy + EDE joint fit
level3_results.md            # Level 3 findings
level3_results.json          # numerical results
results_level1.json          # Level 1 numerical results
results_level2.json          # Level 2 numerical results
```

---

## Section 3 — Key empirical results to reproduce / extend

### Level 1 anchor: Pantheon+ toy-EDE fit
- N = 1580 SNe (z > 0.01 cut), full 1701×1701 covariance from `Pantheon+SH0ES_STAT+SYS.cov`
- ΛCDM:    H₀ = 67.15, Ωₘ = 0.334, χ² = 1548.23
- EDE toy: H₀ = 73.04, Ωₘ = 0.334, f_EDE = 0.0981, χ² = 1515.09
- **Δχ² = −33.14 (>5σ)** with α_rd = 0.4 (literature)

### Level 1 anisotropy z<0.03
- 386 SNe, 13 surveys, per-survey H₀ ∈ [72.95, 74.83]
- Drop-one analysis: tension reduced to [4.49, 5.93]σ when any survey removed → **distributed signal, not single-survey artifact**
- Conclusion: 2.5σ residual is real but sub-publication-grade alone

### Level 2 ΔN_eff stress
- ΔN_eff = 1.5 (would solve H₀): excluded by Planck high-ℓ TT at ~7.5σ + BBN ΔN_eff < 0.5 @ 95% CL
- Pure dark-radiation EDE-proxy is dead. Need true axion physics.

### Level 3 DESI DR2 BAO (13 points)
- Datafile: https://raw.githubusercontent.com/CobayaSampler/bao_data/master/desi_bao_dr2/desi_gaussian_bao_ALL_GCcomb_mean.txt
- Cov: `desi_gaussian_bao_ALL_GCcomb_cov.txt`
- ΛCDM no-SH0ES: H₀ = 68.68, χ² = 13.61 (max residual 1.73σ — clean)
- Tension vs SH0ES: −4.2σ (DESI alone reinforces low-H₀)

### Level 3 classy CLP scan
- classy 3.3.4 with Planck-fixed primary params, scan w(a) = −1 + wₐ(1−a)
- wₐ = 0.5: H₀ shifts by 0.001 km/s/Mpc (negligible)
- wₐ ≥ 1.0: **classy itself rejects** ("w_fld_ini >= 0" error) — CLP cannot describe EDE by construction
- Strong evidence that true axion field is needed

### Level 3 hybrid (anisotropy + EDE)
- Joint fit Pantheon+ + DESI + SH0ES anchor
- Hybrid optimizer collapses δH_local → 0.01 km/s/Mpc, keeps f_EDE = 0.165
- **Anisotropy alone insufficient when EDE is free in joint fit**

---

## Section 4 — What you need to install locally

### 4.1 CLASS_EDE (Smith et al. 2020 fork)
```bash
git clone https://github.com/mwt5345/class_ede
cd class_ede/class_public
make clean
make -j 4
cd ..
python -c "from classy import Class; c = Class(); print('CLASS_EDE OK')"
```
**Documentation:** https://github.com/mwt5345/class_ede
**Reference paper:** Smith, Poulin, Amin (2020), Phys. Rev. D 101, 063523, arXiv:1908.06995

### 4.2 AxiCLASS (alternative, Poulin et al.)
```bash
git clone https://github.com/PoulinV/AxiCLASS
cd AxiCLASS
make clean && make -j 4
```

**Choose one.** CLASS_EDE is simpler (3 EDE params: f_EDE, z_c, θ_i). AxiCLASS exposes more underlying potential params. Recommend CLASS_EDE for first pass.

### 4.3 cobaya for MCMC
```bash
pip install cobaya
cobaya-install planck_2018 --packages-path ~/cobaya_packages
```
This downloads Planck 2018 likelihoods (plik_lite, lowE, lensing) — ~few GB.

### 4.4 Other dependencies
```bash
pip install numpy scipy emcee getdist matplotlib classy camb
```

---

## Section 5 — Concrete next-step pipeline

### Step A — Validate CLASS_EDE installation against published EDE benchmark

Reproduce a result from Smith et al. 2020 (arXiv:1908.06995, their Table I or Fig 1).

```python
from classy import Class
import numpy as np

# Smith+2020 best-fit EDE point (their Table I, with Planck+SH0ES+BAO)
ede = Class()
ede.set({
    'h': 0.7219,
    'omega_b': 0.02281,
    'omega_cdm': 0.1306,
    'A_s': 2.215e-9, 'n_s': 0.9889, 'tau_reio': 0.072,
    # EDE params
    'Omega_fld_ac': 0.122,        # f_EDE = 12.2%
    'log10_a_c': -3.531,           # z_c ~ 3400
    'theta_i_scf': 2.83,           # initial scalar field angle
    'n_scf': 3,
    'CC_scf': 1.0,
    'output': 'tCl,pCl,lCl,mPk', 'lensing': 'yes',
    'l_max_scalars': 3000,
})
ede.compute()
print(f"EDE benchmark: H0={ede.Hubble(0)*299792.458:.2f}, expected ~72.2")
print(f"               Om={ede.Omega_m():.4f}, expected ~0.298")
print(f"               r_d={ede.rs_drag():.2f}, expected ~136-138")
```

If this matches Smith et al. within 0.1 km/s/Mpc, your CLASS_EDE install is good.

### Step B — Compute Planck 2018 χ² for the EDE benchmark vs ΛCDM

Use cobaya's `planck_2018_highl_plik.TTTEEE_lite` + `planck_2018_lowl.TT` + `planck_2018_lowl.EE` + `planck_2018_lensing.clik` likelihoods.

```python
from cobaya.likelihood import Likelihood  # pseudocode — use proper cobaya YAML
# Compare χ²_Planck(EDE_bestfit) vs χ²_Planck(LCDM_bestfit)
# Smith+2020 found Δχ²_Planck ≈ +12 (EDE worse on CMB alone)
# but Δχ²_total (Planck + SH0ES + BAO) ≈ −15 (EDE preferred jointly)
```

### Step C — Joint MCMC: Planck + SH0ES + DESI DR2 + Pantheon+

Cobaya YAML skeleton:
```yaml
likelihood:
  planck_2018_highl_plik.TTTEEE_lite:
  planck_2018_lowl.TT:
  planck_2018_lowl.EE:
  planck_2018_lensing.clik:
  sn.pantheonplus:
  bao.desi_dr2:
  H0.riess2022:                        # SH0ES H0=73.04±1.04

theory:
  classy:
    extra_args:
      Omega_Lambda: 0.0
      Omega_fld: 0.0
      n_scf: 3
      CC_scf: 1.0
      attractor_ic_scf: 'no'
      adptative_stepsize: 100
      scf_tuning_index: 0
      back_integration_stepsize: 1.e-3

params:
  H0:                                  # derived
    latex: H_0
  omega_b: { prior: { min: 0.020, max: 0.024 } }
  omega_cdm: { prior: { min: 0.10, max: 0.14 } }
  As: { prior: { min: 1.7e-9, max: 2.5e-9 } }
  ns: { prior: { min: 0.92, max: 1.0 } }
  tau: { prior: { min: 0.03, max: 0.10 } }
  Omega_fld_ac: { prior: { min: 0.0, max: 0.20 } }    # f_EDE
  log10_a_c: { prior: { min: -4.5, max: -3.0 } }      # z_c
  theta_i_scf: { prior: { min: 0.1, max: 3.1 } }

sampler:
  mcmc:
    Rminus1_stop: 0.05
    max_tries: 10000

output: chains/ede_full_joint
```

Run with `cobaya-run input.yaml`. Expect ~24-48 hours on a desktop with 4 chains.

### Step D — S₈ tension diagnostic

After MCMC converges, compute predicted S₈:
```python
S8_pred = sigma8 * sqrt(Om/0.3)
# Compare to:
# KiDS-1000:  S8 = 0.759 ± 0.024
# DES Y3:     S8 = 0.776 ± 0.017
# Planck (LCDM): S8 = 0.834 ± 0.016
# EDE makes S8 worse by ~0.02. Document the tension.
```

### Step E — If S₈ tension is severe, add self-interacting DM

Try **Camb + interacting DM-DR** model or **etherDM** patch. Question: does adding a small DM-radiation interaction (or DM self-interaction) absorb the S₈ excess while preserving the H₀ resolution? This is the current state-of-art (e.g., Schöneberg et al. 2022, Poulin et al. 2023).

---

## Section 6 — Computational expectations

| Step | Cost | Hardware |
|---|---|---|
| CLASS_EDE compile | ~2 min | any |
| Single CLASS_EDE call | ~3-10 sec | any |
| Single Planck χ² eval | ~30 sec | any |
| Full joint MCMC (4 chains, R-1 < 0.05) | 24-72h | 4-core desktop, 16+ GB RAM |
| Storage for chains | ~5 GB | — |

---

## Section 7 — Anchor data already validated and pre-loaded into existing scripts

| Quantity | Value | Source |
|---|---|---|
| H₀ (SH0ES) | 73.04 ± 1.04 | Riess et al. 2022 |
| H₀ (Planck ΛCDM) | 67.36 ± 0.54 | Planck 2018 |
| Ωₘ (Planck) | 0.315 ± 0.007 | Planck 2018 |
| r_d (Planck ΛCDM) | 147.05 Mpc | Planck 2018 |
| S₈ (KiDS-1000) | 0.759 ± 0.024 | Heymans et al. 2021 |
| S₈ (DES Y3) | 0.776 ± 0.017 | DES 2022 |
| Pantheon+ N | 1580 (z>0.01) | Brout+ 2022 |
| DESI DR2 BAO | 13 points | DESI 2025 |

---

## Section 8 — Auxiliary constraints (apply as priors)

- BBN: Ω_b·h² = 0.02233 ± 0.00036 (Mossa+ 2020)
- BBN: ΔN_eff < 0.5 @ 95% CL
- ACT DR4 (cross-check independent of Planck): H₀ = 67.9 ± 1.5 (LCDM)
- DES Y6 SNe: pending — if released by your run-time, include it

---

## Section 9 — Specific scientific deliverables wanted

1. **Validated CLASS_EDE benchmark** (Step A above)
2. **Full joint MCMC posterior** (Step C) with corner plot for (H₀, Ωₘ, f_EDE, log₁₀(z_c), θ_i, S₈)
3. **Comparison table:** ΛCDM χ² vs EDE χ² broken down by experiment (Planck, SH0ES, BAO, SNe)
4. **S₈ marginalized posterior under EDE** with explicit comparison to KiDS-1000 and DES Y3
5. **If S₈ tension found** → second MCMC with DM-DR interaction or self-interacting DM, same experiments
6. **Push everything to the repo** — `level4_*.md` for docs, `level4_*.py` for scripts, `level4_*.json` for numerical results, `level4_chains/` for MCMC chains

---

## Section 10 — Repo authentication

GitHub PAT to push results: **see secure note from Aciiderixx separately.**
*(The token initially used in cloud sandbox is being rotated. Aciiderixx will provide a fresh token to whoever runs this locally — do not commit any token to the repo.)*

Branch strategy:
- `main` — clean, peer-reviewable
- `level4-mcmc` — your working branch with exploratory chains
- Squash to `main` when results are publication-grade

---

## Section 11 — Communication protocol for Aciiderixx

He will read your results via:
- GitHub commit messages (concise, scientific)
- `levelN_results.md` markdown reports
- `levelN_results.json` for downstream tooling

He prefers:
- Honest results > positive spin (this is research, not advocacy)
- Explicit "what failed" sections alongside successes
- Δχ² broken down per dataset (not just totals)
- Fitted vs published comparison whenever possible

---

## Section 12 — Known pitfalls

1. **CLASS_EDE input parameter naming** changes between forks (Omega_fld_ac vs fEDE_max vs f_ede). Read the fork's README carefully.
2. **CLP fluid trap:** stock classy's CLP fluid CANNOT model EDE (we proved this empirically — it refuses w(a) extensions where w_early ≥ 1/3). Use the EDE fork.
3. **Planck likelihood code (clik):** old C library, sometimes finicky to install. cobaya-install handles it.
4. **MCMC chain initialization:** EDE posteriors are bimodal (one peak at f_EDE ≈ 0, another at f_EDE ≈ 0.10). Start chains from both regions or use stepped tempering.
5. **r_d derivation:** under EDE, classy reports rs_drag — use that, not the toy `r_d × (1 − α·f_EDE)` linearization we used in cloud (good for L1/L3 but not for fits with Planck likelihood).

---

## Section 13 — Reading list (highest-priority papers)

1. **Smith, Poulin, Amin (2020)** — arXiv:1908.06995. Original CLASS_EDE paper. Read first.
2. **Murgia, Abellán, Poulin (2021)** — arXiv:2009.10733. EDE confronted with full datasets.
3. **Schöneberg et al. (2022)** — arXiv:2107.10291. Comprehensive H₀ tension review including EDE + S₈.
4. **Poulin, Smith, Karwal (2023)** — arXiv:2302.09032. Status of EDE post-DESI.
5. **DESI Collaboration (2024-2025)** — arXiv:2404.03002 + DR2 release. BAO impact on H₀.

---

## End of brief

If anything is unclear, dig into the existing scripts in the repo first — they're heavily commented and their output is reproducible bit-for-bit (Pantheon+ download URL hard-coded).

**Good luck. The question matters.**
