# Adversarial Machine Learning Security Assessment (AICS-202)

End-to-end adversarial security evaluation of a supervised classification model (Logistic Regression on `scikit-learn` `digits`), aligned with the **NIST AI 100-2e2025** taxonomy. This repository covers baseline threat modeling, inference-time evasion (FGSM), training-time label poisoning, and adaptive defense evaluation (FGSM & PGD).

## Repository Structure

| File | Description |
| :--- | :--- |
| `AICS-202_Adversarial Machine Learning_Report...` | Consolidated technical assessment report, threat model, integrity controls, and governance decision memo. |
| `AICS202_Lab01_Baseline_Threat_Model.ipynb` | Baseline model training (96.2% clean accuracy), confusion matrix analysis, and scoped threat model. |
| `AICS202_Lab02_FGSM_Evasion.ipynb` | White-box $L_\infty$ FGSM evasion across $\epsilon \in \{0.00, 0.03, 0.07, 0.12, 0.15, 0.18\}$ with visual inspection and clipping analysis. |
| `AICS202_Lab03_Poisoning_Data_Integrity.ipynb` | Controlled random training-label corruption (0% to 20%) and data pipeline integrity controls. |
| `AICS202_Lab04_Robustness_Defences.ipynb` | Adversarial augmented training ($\epsilon=0.10$ and student variant $\epsilon=0.05$) evaluated under model-specific adaptive FGSM/PGD attacks. |

## Key Empirical Results

* **Baseline (Lab 01):** `96.2%` clean test accuracy (1,347 train / 450 test stratified split).
* **Evasion (Lab 02):** FGSM accuracy drops to `51.6%` at $\epsilon=0.12$, `36.0%` at $\epsilon=0.15$, and `20.4%` at $\epsilon=0.18$.
* **Poisoning (Lab 03):** `20%` random label corruption (269 labels altered) reduces clean accuracy to `93.3%`, demonstrating the gap between random noise and targeted backdoors.
* **Adaptive Defenses (Lab 04):**
  * **Baseline (Undefended):** Clean `96.2%` | FGSM `51.6%` | PGD `48.9%`
  * **Augmented Training A ($\epsilon=0.10$):** Clean `94.0%` | FGSM `28.4%` | PGD `26.0%` *(Rejected)*
  * **Student Variant B ($\epsilon=0.05$):** Clean `95.8%` | FGSM `43.3%` | PGD `40.2%` *(Rejected)*
* **Governance Decision:** Augmented training is **rejected** for this linear classifier due to persistent negative outcomes under adaptive testing. Recommended mitigations prioritize dataset integrity controls (cryptographic hashing, RBAC, versioning) and inference-time defense-in-depth.

## Environment & Reproducibility

* **Execution Environment:** Google Colab (Cloud-hosted Jupyter Notebook)
* **Python Version:** Python 3.x (Colab default runtime)
* **Core Dependencies:** `scikit-learn`, `numpy`, `matplotlib`
* **Random Seeds:** `random_state=42`, `rng=2026`

### How to Reproduce
1. Open each `.ipynb` notebook in Google Colab in numerical order (Lab 01 to Lab 04).
2. Run **Runtime** $\rightarrow$ **Restart session and run all** to reproduce all metrics and charts.
3. The original `scikit-learn` `digits` dataset remains offline and unchanged; only the training partition is modified in Lab 03.

## References
* NIST AI 100-2e2025 — *Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations* (March 2025)
* MITRE ATLAS — *Adversarial Threat Landscape for Artificial-Intelligence Systems*
