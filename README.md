# Using-LLM-to-predict-transmissibility-and-virulence-of-ARI-strains-based-on-RNA-structure

## Description

This project uses **Protein Language Models (ESM-2)** to predict epidemiological parameters of SARS-CoV-2 variants directly from the spike protein amino acid sequence:

- **Transmissibility** — basic reproduction number **R₀**
- **Virulence** — two coefficients:
  - **Zvoda1** = Deaths / Hospitalizations
  - **Zvoda2** = Hospitalizations / All cases

The system consists of **two independent regression models** trained separately for maximum accuracy.

## Data

Synthetic sequences generated from real mutations (GISAID/Nextstrain) for 5 variants:

| Variant | Samples | Role |
| :--- | :--- | :--- |
| BA.1, BA.2, BA.5 | 600 | Train |
| XBB.1.5, JN.1 | 400 | Test (holdout) |

## Model Architecture

Uses **ESM-2** (35M parameters, `esm2_t12_35M_UR50D`) as backbone.

- **Model 1 (R₀):** Single-task regression head
- **Model 2 (Virulence):** Two independent heads for Zvoda1 and Zvoda2 (multi-task)

## Results (Holdout: XBB.1.5, JN.1)

**Transmissibility:**

| Variant | True R₀ | Pred R₀ | Error |
| :--- | :--- | :--- | :--- |
| XBB.1.5 | 1.713 | 1.388 | 19.0% |
| JN.1 | 2.154 | 1.442 | 33.0% |

**Virulence:**

| Variant | True Zvoda1 | Pred Zvoda1 | Error |
| :--- | :--- | :--- | :--- |
| XBB.1.5 | 0.1361 | 0.1435 | 5.4% |
| JN.1 | 0.1400 | 0.1427 | 1.9% |


## Limitations

- Synthetic data (real GISAID validation needed)
- Only spike protein analyzed
- Underestimation of R₀ for new variants (extrapolation issue)
