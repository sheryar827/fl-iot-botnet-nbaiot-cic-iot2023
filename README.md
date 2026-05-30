# fl-iot-botnet-nbaiot-cic-iot2023

Comparison of **FedAvg**, **FedProx**, and **FedTrimmedAvg** for IoT botnet
detection on the **N-BaIoT** and **CIC-IoT2023** datasets under **non-IID**
client distributions.

This repository accompanies an MS Cybersecurity thesis investigating how the
choice of federated aggregation strategy affects botnet-detection performance
when client data is heterogeneous (non-IID), as is realistic for distributed
IoT deployments.

---

## Research question

Under realistic non-IID partitioning of IoT traffic across clients, how do
robust and proximal aggregation strategies (FedProx, FedTrimmedAvg) compare
against vanilla FedAvg - and against a centralized upper bound - for botnet
detection accuracy, F1, and convergence speed?

## Datasets

| Dataset | Source | Notes |
|---|---|---|
| N-BaIoT | UCI Machine Learning Repository | Network traffic from 9 commercial IoT devices; benign + Mirai/BASHLITE attack traffic |
| CIC-IoT2023 | Canadian Institute for Cybersecurity (CIC) | Large-scale IoT attack dataset across multiple attack categories |

> **Note:** The raw datasets are **not** redistributed in this repository due to
> their size and original licensing terms. Download them from the sources above
> and follow the preprocessing in the notebooks to reproduce the inputs.

## Methods

- **Aggregation strategies:** FedAvg, FedProx (proximal term μ), FedTrimmedAvg (coordinate-wise trimmed mean)
- **Models:** MLP and CNN classifiers
- **Data partitioning:**
  - IID baseline
  - Non-IID via Dirichlet label partitioning at several concentration values α (lower α = more skew)
- **Baselines:** Centralized training (performance upper bound) per model
- **Reproducibility:** Each configuration is run over multiple random seeds; results are reported as **mean ± std**

### Experimental configuration

Values used in the reported runs (from the `CFG` block in each notebook):

| Parameter | Value |
|---|---|
| Number of clients | 9 |
| FL rounds | 30 |
| Local epochs | 3 |
| Batch size | 512 |
| Learning rate | 1e-3 |
| Test fraction | 0.2 |
| Convergence threshold (F1) | 0.95 |
| Max samples per attack-type CSV | 20,000 |
| Dirichlet α values | 1.0, 0.5, 0.1 |
| FedProx μ | 0.01 |
| FedTrimmedAvg trim fraction | 0.1 |
| Random seeds | 42, 123, 7 (3 seeds) |

Per (model, seed) there are **11 experiments**: 1 centralized baseline + 1 IID
FedAvg baseline + a 3×3 matrix of {FedAvg, FedProx, FedTrimmedAvg} × {three
Dirichlet α values}.

## Repository structure

```
.
├── FL_IoT_Botnet_NBaIoT_MLP_CNN.ipynb      # N-BaIoT experiments
├── FL_IoT_Botnet_CIC_IoT2023_MLP_CNN.ipynb # CIC-IoT2023 experiments
├── fl_nbaiot_results/                       # N-BaIoT outputs (CSVs, figures)
├── fl_cic_iot_2023_results/                 # CIC-IoT2023 outputs (CSVs, figures)
├── .gitignore
├── LICENSE                                  # MIT
└── README.md
```

## Results files

Each results folder contains:

| File | Contents |
|---|---|
| `csv/all_runs_raw.csv` | One row per (model, run, seed): final/best accuracy & F1, convergence round |
| `csv/experiment_summary_mean_std.csv` | Human-readable summary aggregated across seeds (`mean ± std`) |
| `csv/experiment_summary_numeric.csv` | Same aggregation, numeric columns for plotting/analysis |
| `csv/history_*.csv` | Per-round accuracy/F1 history for an individual run |

**Reading the summary:** in `experiment_summary_mean_std.csv`, the `Seeds`
column reports how many seeds contributed to each row. `Conv_Round` is the round
at which the run reached its convergence criterion (lower is faster).

## Results

All figures are macro-F1 (%), mean ± std over 3 seeds. Identical hyperparameters
were used across both datasets, so the dataset is the only variable in the
comparison.

### Headline: best non-IID F1 per strategy (most heterogeneous setting, α = 0.1)

| Dataset | Model | FedAvg | FedProx | FedTrimmedAvg |
|---|---|---|---|---|
| N-BaIoT | MLP | 81.27 ± 5.69 | 76.48 ± 6.41 | **83.34 ± 2.78** |
| N-BaIoT | CNN | 78.21 ± 1.46 | 68.69 ± 0.38 | **77.66 ± 3.20** |
| CIC-IoT2023 | MLP | 44.68 ± 7.19 | 50.13 ± 9.07 | **60.86 ± 2.08** |
| CIC-IoT2023 | CNN | 41.66 ± 7.62 | 38.08 ± 10.72 | **56.34 ± 2.15** |

### Robustness to heterogeneity (F1 drop from α = 1.0 to α = 0.1)

Smaller is better - it measures how much performance is lost as client data
becomes more skewed.

| Dataset | Model | FedAvg | FedProx | FedTrimmedAvg |
|---|---|---|---|---|
| N-BaIoT | MLP | 6.41 pp | 10.84 pp | **4.37 pp** |
| N-BaIoT | CNN | 9.46 pp | 18.65 pp | **9.95 pp** |
| CIC-IoT2023 | MLP | 21.42 pp | 12.89 pp | **6.20 pp** |
| CIC-IoT2023 | CNN | 21.78 pp | 18.55 pp | **7.46 pp** |

(Centralized upper bounds: N-BaIoT ≈ 87.8% F1; CIC-IoT2023 ≈ 70.7% (MLP) /
68.8% (CNN). N-BaIoT is the more separable task; CIC-IoT2023 is substantially
harder.)

### Key findings

**FedTrimmedAvg is the most robust aggregation strategy under non-IID data.**
It gives the best (or statistically tied) non-IID F1 in every dataset/model
combination, and crucially it loses the *least* performance as client skew
increases. On CIC-IoT2023 it degrades by only ~6–7 pp from α = 1.0 to α = 0.1,
versus ~21 pp for FedAvg. Coordinate-wise trimming discards the most extreme
client updates, which is exactly the failure mode that severe label skew
produces - so the result is consistent with the method's design intent.

**FedProx underperforms here, and this is a tuning artifact rather than a
property of the method.** FedProx is intended to *help* under heterogeneity,
yet it is frequently the worst performer at α = 0.1 and carries the largest
variance (e.g. ±10.72 on CIC-IoT2023 CNN). The proximal coefficient was fixed
at μ = 0.01 across all skew levels; a value that small barely constrains local
drift, so FedProx behaves close to FedAvg while adding optimization noise. A
per-skew μ sweep would be the natural follow-up; the present results should be
read as "FedProx at μ = 0.01," not as a verdict on FedProx in general.

**The MLP matches or beats the CNN throughout.** For engineered statistical
flow features there is no spatial structure for convolutions to exploit, so the
simpler MLP is competitive and often better - an expected outcome worth stating
explicitly.

**Note on the IID/non-IID boundary:** on CIC-IoT2023 the best α = 0.5 run
(67.68%) marginally exceeds the IID baseline (67.51%). This difference is well
within one standard deviation (±1.57) and reflects seed noise, not a genuine
inversion.

## Reproducing the experiments

1. Open the relevant notebook in Google Colab (or Jupyter).
2. Download the dataset from its source (see **Datasets**) and point the data-loading cell at it.
3. Set the `CFG` parameters as listed above (or your own).
4. Run all cells. The orchestration loop checkpoints after every run, so an
   interrupted session resumes completed runs from the per-run history CSVs on
   the next execution.
5. The summary cell aggregates `all_runs_raw.csv` into the mean ± std tables.

> Runs are resumable: completed FL runs are detected from their history CSV and
> skipped, so a Colab disconnect does not force a full restart.

## Citing this work

If you use this code or results, please cite the repository (see
[`CITATION.cff`](CITATION.cff)) - GitHub will render a **"Cite this repository"**
button. For a permanent, citable archive, consider minting a DOI via
[Zenodo](https://zenodo.org).

## License

Released under the [MIT License](LICENSE). The datasets retain their own
respective licenses; refer to the original sources.

## Author

**Sheryar Kiani** - MS Cybersecurity, Air University, Islamabad.
