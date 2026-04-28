# CLAUDE_HANDOFF — Updated April 28, 2026 (Level 5 erratum applied)

## ⚠️ Read LEVEL5_ERRATUM.md FIRST

The "f(R) wins on H0+S8" conclusion from Level 4 (Step D) was based on a naive σ8 scaling. When σ8 is computed properly via the growth ODE + Eisenstein-Hu transfer (Level 5 Step E), the result inverts: f(R) at Ωm=0.260 gives S8≈0.59 (not 0.78), which is −9σ vs KiDS/DES.

## Honest current status

- **Level 1–3:** Background fits valid (Pantheon+ + DESI + CC + SH0ES anchor reject ΛCDM at Δχ²~24).
- **Level 4 Step B:** Valid. r_d shrinks to 139.78 Mpc agnostically — strong EDE-like signature.
- **Level 4 Steps C, D:** Background fits valid; σ8/S8 column is WRONG.
- **Level 5:** σ8 corrected. Late-time MG cannot solve H0+S8 simultaneously with Ωm≈0.31.

## Real thesis to defend

EDE-like pre-recombination physics is preferred for H0. Tradeoff: ~2-3σ pull on S8 (modest, not catastrophic). This matches the published consensus (Hill+2020, Murgia+2021, Smith+2022).

## What's needed beyond sandbox

1. CLASS_EDE or AxiCLASS local install (gcc/make).
2. cobaya MCMC: full CMB likelihood + Pantheon+ + BAO + KiDS/DES.
3. Interacting DE and decaying DM as alternative late-time mechanisms.
4. If anything survives the joint fit: arxiv preprint.

## Repo state

- LEVEL4_RESULTS.md: keep, but cross-reference erratum
- LEVEL5_ERRATUM.md: critical correction (Apr 28)
- level5_data.json: Step E numbers
- CLAUDE_HANDOFF.md: this file
