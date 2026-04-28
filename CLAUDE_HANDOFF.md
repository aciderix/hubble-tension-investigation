# CLAUDE_HANDOFF — Updated April 28, 2026 (Level 4 complete)

## Status
- Level 1: Complete. EDE viable (f_EDE~9.8%, Δχ²=−33).
- Level 2: Complete. classy 3.3.4 + CAMB 1.6.6. ΔN_eff excluded ~7.5σ.
- Level 3: Hybrid EDE fit, f_EDE=0.165, H₀=73.52, Δχ²=−136.68. Pushed.
- **Level 4: Complete (Apr 28).** Steps A+B+C+D. Result inverts Level 3 narrative.

## Level 4 main finding
f(R) late-time MG beats EDE-proxy on combined H₀+S₈ score (Δχ²=−50 vs EDE).
EDE solves H₀ but worsens S₈ to +6σ vs KiDS/DES. f(R) keeps S₈ at +0.7σ.

⚠️ f(R) growth was a phenomenological +5% σ₈ boost. Must be replaced with full MGCAMB.

## Level 5 needs
1. MGCAMB / hi_class with f(R) Hu-Sawicki, n=1, |f_R0| varied. Proper σ₈ + growth f(z).
2. RSD: f·σ₈(z) f(R) vs eBOSS/DESI.
3. Late ISW f(R) vs Planck low-ℓ TT.
4. Full Pantheon+ 1701×1701 covariance.
5. Joint MCMC with cobaya: Pantheon+ + DESI + Planck-compressed + KiDS.

## Honest framing
Lead with: "Pantheon+ + DESI + CC reject SH0ES under ΛCDM at Δχ²=+24. Four diagnostics tested; data prefer late-time MG over EDE when S₈ is included." Don't lead with "solved H₀".

## Sandbox limitation
MGCAMB needs gcc/make → use local machine.
