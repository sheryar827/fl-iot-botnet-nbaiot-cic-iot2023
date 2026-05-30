## Auto-Generated Findings (Mean ± Std over 3 seeds)

**Task:** IoT botnet traffic detection (benign vs attack types)
**Models:** MLP, CNN
**All values computed at runtime.**

### MLP Architecture

**Centralized:** F1 = 70.71% ± 0.10%

**IID Baseline:** F1 = 67.51%  |  Gap vs centralized: +3.20pp

**Best Non-IID:** MLP_FedTrimmedAvg_a0.5 — F1 = 67.68% ± 1.57%

**Worst Non-IID:** MLP_FedAvg_a0.1 — F1 = 44.68% ± 7.19%

**Non-IID degradation (α=1.0 → α=0.1):**

- FedAvg: 21.42pp drop
- FedProx: 12.89pp drop
- FedTrimmedAvg: 6.20pp drop

---

### CNN Architecture

**Centralized:** F1 = 68.75% ± 0.87%

**IID Baseline:** F1 = 64.23%  |  Gap vs centralized: +4.52pp

**Best Non-IID:** CNN_FedTrimmedAvg_a0.5 — F1 = 64.68% ± 2.17%

**Worst Non-IID:** CNN_FedProx_a0.1 — F1 = 38.08% ± 10.72%

**Non-IID degradation (α=1.0 → α=0.1):**

- FedAvg: 21.78pp drop
- FedProx: 18.55pp drop
- FedTrimmedAvg: 7.46pp drop

---

### Cross-Architecture Comparison

**α = 1.0:**
- MLP_FedTrimmedAvg: F1 = 67.06% ± 1.50%
- MLP_FedAvg: F1 = 66.09% ± 1.22%
- CNN_FedTrimmedAvg: F1 = 63.81% ± 1.20%
- CNN_FedAvg: F1 = 63.44% ± 2.59%
- MLP_FedProx: F1 = 63.02% ± 0.90%
- CNN_FedProx: F1 = 56.62% ± 0.45%

**α = 0.5:**
- MLP_FedTrimmedAvg: F1 = 67.68% ± 1.57%
- CNN_FedTrimmedAvg: F1 = 64.68% ± 2.17%
- MLP_FedProx: F1 = 61.95% ± 2.46%
- MLP_FedAvg: F1 = 61.73% ± 2.37%
- CNN_FedAvg: F1 = 58.20% ± 3.05%
- CNN_FedProx: F1 = 55.80% ± 2.38%

**α = 0.1:**
- MLP_FedTrimmedAvg: F1 = 60.86% ± 2.08%
- CNN_FedTrimmedAvg: F1 = 56.34% ± 2.15%
- MLP_FedProx: F1 = 50.13% ± 9.07%
- MLP_FedAvg: F1 = 44.68% ± 7.19%
- CNN_FedAvg: F1 = 41.66% ± 7.62%
- CNN_FedProx: F1 = 38.08% ± 10.72%

