# Digital Twin of Microfluidic Gastruloid Symmetry Breaking & Morphogenesis

This dry-lab modelling project builds a mechanistic in silico framework directly extending experimental microfluidics research conducted in the Laboratory of Stem Cell Bioengineering (LSCB) at EPFL (Study of Gastruloid Self-organization through Spatiotemporal Control of Wnt Activation Using Microfluidics).


## Introduction 

Gastruloids are embryonic organoids formed by aggregated embryonic stem cells with similar morphological change with early stage of embryo development, which makes a good model system for the study of mammalian development and disease. [^1] During the early development stage of the embryo, Wnt signaling coordinates cell polarity, regulates tissue morphogenesis, and plays an important role in the establishment of anteroposterior (AP) axis, which is crucial for embryo development. However, it remains unclear how the elongation axis and extent are regulated, as well as the formation of anterior neural tissue, fore-, mid-, and hindbrain, are still lacking.

Fig1. Elongation of gastruloid in vitro [^1]
![Literature](figures/Fig1_Elongation_of_gastruloid.png)

To guide deterministic AP axis formation via localized chemical gradients, three generations of PDMS microfluidic devices were designed and fabricated, culminating in Chip No. 3—a $1,000\ \mu\text{m}$ diameter, $490\ \mu\text{m}$ high central trapping chamber flanked by $1\%$ PEG hydrogel diffusion barriers, enlarged media reservoirs, and a non-adhesive $2\%$ Pluronic-F127 coating into which gastruloids were transferred at $48\ \text{h}$ after aggregation (AA). Across baseline 96-well perturbation assays and on-chip culture experiments, four empirical observations revealed tight coupling between morphogen transport, device physics, and tissue morphogenesis:

**Narrow Biphasic CHIR99021 Window:** Screening concentrations from 0.2x ($0.474\ \mu\text{M}$) to 5x ($11.85\ \mu\text{M}$) relative to standard 1x ($2.37\ \mu\text{M}$) across $75\ \mu\text{L}$ and $150\ \mu\text{L}$ well volumes, alongside $18\text{–}48\ \text{h}$ pulse durations, showed that elevated doses ($3\times\text{–}5\times$) or prolonged $48\ \text{h}$ exposure suppressed elongation, shortened aggregates by $144\ \text{h}$ AA, or caused cell death.

**Cell-Line Morphological Divergence (TLC vs. SBr):** Evaluated via ImageJ Maximum Inscribed Circle fitting, SBr (T/Bra::mCherry) and TLC (TCF/LEF::mCherry) aggregates displayed distinct axial geometries in 96-well plates (SBr reaching greater total axial length yet often lower classical Elongation Index), while under homogeneous 24h on-chip CHIR pulses ($0.5\times = 1.18\ \mu\text{M}$ and $0.7\times = 1.66\ \mu\text{M}$), TLC elongated by $144\ \text{h}$ AA whereas SBr failed to elongate significantly.

**Sensitivity to Incomplete Medium Washout:** Exchanging only $50\ \mu\text{L}$ rather than the full $150\ \mu\text{L}$ well volume after the CHIR pulse impaired $144\ \text{h}$ AA axial extension, indicating that residual post-pulse CHIR retention and metabolic waste accumulation inhibit late-stage elongation.

Fig2. Effect of chiron concentration and culture volume on gastruloids a. Axial morphogenesis of gastruloids at 144h AA. TLC (TCF/LEF:: mCherry), SBr (T/Bra:mCherry).

![CHR_effect](figures/Fig2_Effect_of_CHR_conc_and_volume.png)

**Loss of Directional Polarization in Static Microfluidic Gradients:** Plugging the Chip No. 3 outlet with $2.5\%$ PEG hydrogel to isolate a CHIR-supplemented left channel from a fresh-medium right channel induced transient SBr elongation at $96\ \text{h}$ AA that vanished by $120\ \text{h}$ AA, which was hypothesized to result from free rotational Brownian motion scrambling gastruloid orientation inside the chamber.

Fig3. Axial morphogenesis of gastruloids in chip No.3.
![Gastruloid-on-a-chip](figures/Fig3_Gastruloid_in_the_chip.png)

## In silico simulation to test experimental hypothesis

Although the experimental study proposed integrating pump-driven perfusion in future work to supply fresh medium and stabilize the biochemical gradient, disentangling diffusive bottlenecks, rotational mechanics, and shear-safe flow rates purely through wet-lab iteration is slow and resource-intensive. This repository translates those 96-well and Chip No. 3 setups into a four-stage Python simulation pipeline—combining medial-axis bioimage morphometry, multi-compartment finite-volume transport, coupled Turing reaction–diffusion and rotational SDEs, and 2D advection–diffusion hydrodynamics—to test each experimental hypothesis from first principles and prescribe optimal microfluidic operating parameters.

1. **Cell-Line Morphometry (`SBr` vs. `TLC`):** Classical Elongation Index ($L/D_{\max}$) underestimates `SBr` (`T/Bra::mCherry`) axial extension because `SBr` retains a bulbous anterior head alongside a narrow posterior tail, whereas `TLC` (`TCF/LEF::mCherry`) narrows globally with C-axis bending. Medial-axis Euclidean Distance Transform profiling ($D(s)$, Mean-Width EI, AP Tapering $\mathcal{T}_{AP}$, and Tail Fraction) resolves this paradox.
   ![Morphometry](figures/fig1_sbr_vs_tlc_morphology.png)
3. **CHIR99021 Transport & Ideal Dosing (96-Well vs. Static Chip):** Unmixed 96-well vertical gradients disappear in **~3.4 h** ($150\ \mu\text{L}$) or **~0.85 h** ($75\ \mu\text{L}$) at $1.0\times C_{\text{applied}}$. In static Chip No. 3, narrow supply channels bottleneck diffusion—erasing **>75% of the cross-gastruloid gradient within ~8–12 h**, equilibrating to **$0.50\times C_{\text{left}}$**, and delaying post-pulse washout (selectively arresting `SBr`). Joint biphasic modeling identifies **$C_{\text{ideal}} = 1.35\ \mu\text{M}$** ($C_{\text{left}} = 2.70\ \mu\text{M}$) as optimal.
   ![Equilibration](figures/fig2_chir_equilibration_and_optimum.png)
4. **Biophysical Test of the Rotational Brownian Motion Hypothesis:** Stokes–Einstein–Debye thermal rotational diffusion of a $R = 125\ \mu\text{m}$ gastruloid ($D_{\text{rot}}^{\text{thermal}} = 4.55\times 10^{-4}\ \text{rad}^2/\text{h}$, $8.5^\circ$ RMS in 24 h) is **too slow** to disrupt gradient locking. Instead, zero-adhesion Pluronic-F127 coating permits active cellular migration and pipette-handling tumbling ($D_{\text{rot}}^{\text{eff}} \sim 0.8\ \text{rad}^2/\text{h}$), randomizing AP pole orientation.
   ![Brownian Test](figures/fig3_brownian_hypothesis_test.png)
5. **Microfluidic Perfusion & Hydrodynamic Pinning Design:** 2D advection–diffusion and Stokes/gap-suction mechanics show that flanking perfusion at **$Q_{\text{side}} \ge 25\text{–}30\ \mu\text{L}/\text{h}$** establishes a $>95\%$ constant gradient within **~36 min** ($95\%$ steady state), while a gentle central pillar-trap holding flow of **$Q_{\text{hold}} = 2\text{–}6\ \mu\text{L}/\text{h}$** prevents gastruloid rotation ($<5^\circ$ wobble) without diluting the CHIR gradient or exceeding stem-cell shear limits ($<2.5\ \text{mPa}$).
   ![Flow Design](figures/fig4_flow_and_pinning_design.png)



[^1]: Beccari, L., Moris, N., Girgin, M. et al. Multi-axial self-organization properties of mouse embryonic stem cells into gastruloids. Nature 562, 272–276 (2018).
