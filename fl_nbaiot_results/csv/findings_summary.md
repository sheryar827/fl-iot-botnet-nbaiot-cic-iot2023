## Auto-Generated Findings (Mean ± Std over 3 seeds)

**Task:** IoT botnet traffic detection (benign vs attack types)
**Models:** MLP, CNN
**All values computed at runtime.**

### MLP Architecture

**Centralized:** F1 = 87.84% ± 0.00%

**IID Baseline:** F1 = 87.75%  |  Gap vs centralized: +0.09pp

**Best Non-IID:** MLP_FedTrimmedAvg_a1.0 — F1 = 87.71% ± 0.08%

**Worst Non-IID:** MLP_FedProx_a0.1 — F1 = 76.48% ± 6.41%

**Non-IID degradation (α=1.0 → α=0.1):**

- FedAvg: 6.41pp drop
- FedProx: 10.84pp drop
- FedTrimmedAvg: 4.37pp drop

---

### CNN Architecture

**Centralized:** F1 = 87.85% ± 0.00%

**IID Baseline:** F1 = 87.61%  |  Gap vs centralized: +0.24pp

**Best Non-IID:** CNN_FedAvg_a1.0 — F1 = 87.67% ± 0.07%

**Worst Non-IID:** CNN_FedProx_a0.1 — F1 = 68.69% ± 0.38%

**Non-IID degradation (α=1.0 → α=0.1):**

- FedAvg: 9.46pp drop
- FedProx: 18.65pp drop
- FedTrimmedAvg: 9.95pp drop

---

### Cross-Architecture Comparison

**α = 1.0:**
- MLP_FedTrimmedAvg: F1 = 87.71% ± 0.08%
- MLP_FedAvg: F1 = 87.67% ± 0.08%
- CNN_FedAvg: F1 = 87.67% ± 0.07%
- CNN_FedTrimmedAvg: F1 = 87.61% ± 0.01%
- CNN_FedProx: F1 = 87.35% ± 0.15%
- MLP_FedProx: F1 = 87.32% ± 0.25%

**α = 0.5:**
- MLP_FedTrimmedAvg: F1 = 87.51% ± 0.19%
- CNN_FedAvg: F1 = 87.41% ± 0.25%
- MLP_FedAvg: F1 = 87.39% ± 0.55%
- CNN_FedTrimmedAvg: F1 = 87.32% ± 0.42%
- MLP_FedProx: F1 = 86.85% ± 0.48%
- CNN_FedProx: F1 = 86.52% ± 0.70%

**α = 0.1:**
- MLP_FedTrimmedAvg: F1 = 83.34% ± 2.78%
- MLP_FedAvg: F1 = 81.27% ± 5.69%
- CNN_FedAvg: F1 = 78.21% ± 1.46%
- CNN_FedTrimmedAvg: F1 = 77.66% ± 3.20%
- MLP_FedProx: F1 = 76.48% ± 6.41%
- CNN_FedProx: F1 = 68.69% ± 0.38%

