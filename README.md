# GPU Underutilization Candidate Analysis using MITSuperCloud 

## Overview

This project studies GPU underutilization patterns in a subset of MIT
SuperCloud telemetry data. It combines an initial exploratory data analysis
(EDA) with a calibrated, device-aware framework for identifying
telemetry-defined underutilization candidates.

The framework does not prove that a job was wasteful or that its energy is
recoverable. It reports reproducible candidates for operational investigation
and the measured energy associated with them.

## Dataset & Acknowledgments
This project utilizes dcgm and scheduler data from the **MIT SuperCloud Dataset**, released as part of the following HPCA '22 publication:
> **AI-Enabling Workloads on Large-Scale GPU-Accelerated System: Characterization, Opportunities, and Implications**<br>
> Baolin Li, Rohin Arora, Siddharth Samsi, Tirthak Patel, et al.<br>
> *2022 IEEE International Symposium on High-Performance Computer Architecture (HPCA)*

## From EDA to a calibrated framework

The initial EDA used fixed, cluster-wide heuristic rules to identify broad
phantom-load patterns. It was a useful first pass, but fixed thresholds can be
fragile: idle power varies across devices, high PCIe traffic can be legitimate
I/O, and a zero SM-utilization average does not by itself establish job intent.

`Framework.ipynb` addresses these limitations by:

1. separating zero-compute observations from active workloads;
2. defining low compute from the active-workload distribution;
3. estimating idle-power references per `(Node, gpu_id)` with robust MAD;
4. treating I/O as workload context rather than automatic waste;
5. reporting measured DCGM energy associated with candidate categories.

## Results

The EDA and framework are complementary rather than contradictory.

| Analysis level | Jobs flagged | Energy associated with flagged jobs |
|---|---:|---:|
| Initial EDA: broad fixed-rule phantom-load patterns | 43.71% | 13.92% |
| Framework: broad candidate categories, including I/O-related cases | ~35.7% | ~12.41% |
| Framework: stricter energy-risk candidates | ~6.9% | ~4.23% |

The broad framework results are similar to the original EDA, suggesting that
the initial analysis identified a meaningful concentration of low-utilization
patterns. The stricter category excludes or separates ambiguous cases such as
I/O-related workloads. It is the most defensible telemetry-defined subset, but
it still requires external validation before being described as confirmed waste
or recoverable energy.

The dataset subset contains 96,893 GPU job records. The stricter energy-risk
group is associated with approximately 112 kWh of **measured** energy.

## Methodology

The framework uses the following calibrated rules:

1. **Zero-compute separation:** `SM utilization == 0` is handled as a distinct
   telemetry state.
2. **Low-compute calibration:** the threshold is the 10th percentile of
   non-zero SM-utilization observations.
3. **Quiet idle reference:** local idle power is estimated from zero-compute,
   low-memory, low-I/O observations for each `(Node, gpu_id)` group.
4. **Elevated-power detection:** power above the local idle-power median plus
   three median absolute deviations (MAD) is treated as elevated.
5. **I/O context:** high PCIe activity is an I/O-bound candidate category, not
   proof of inefficiency.
6. **Energy reporting:** energy is calculated from DCGM
   `energyconsumed_joules`; the repository does not estimate avoidable energy.

## Contents

- **Main.ipynb**: Initial EDA and broad fixed-rule heuristic analysis.
- **Framework.ipynb**: Calibrated, device-aware underutilization-candidate
  framework.

To run the framework, place `dcgm.csv` in `1. Data/`, restart the notebook
kernel, and run all cells from top to bottom.

## Limitations

- The analysis uses job-level average telemetry, not time-series activity.
- Candidate categories have not yet been validated against scheduler logs,
  application logs, GPU process information, or manual review.
- The framework has currently been applied to one dataset only.
- P10, P25, and MAD × 3 are reasonable calibration choices, not proven
  universally optimal thresholds.
- Relative PCIe activity is not equivalent to physical PCIe saturation without
  verified GPU and PCIe hardware metadata.

## Next Steps

1. Validate a random sample of each candidate category against independent job
   and scheduler information.
2. Extend threshold sensitivity analysis beyond the idle-power MAD multiplier.
3. Apply the framework to a second GPU dataset or cluster trace.
4. Add verified hardware metadata for physical I/O comparisons.

## Citation
If you use or reference this repository, please cite the original paper using the BibTeX entry below:

```bibtex
@INPROCEEDINGS{9773216,
  author={Li, Baolin and Arora, Rohin and Samsi, Siddharth and Patel, Tirthak and Arcand, William and Bestor, David and Byun, Chansup and Roy, Rohan Basu and Bergeron, Bill and Holodnak, John and Houle, Michael and Hubbell, Matthew and Jones, Michael and Kepner, Jeremy and Klein, Anna and Michaleas, Peter and McDonald, Joseph and Milechin, Lauren and Mullen, Julie and Prout, Andrew and Price, Benjamin and Reuther, Albert and Rosa, Antonio and Weiss, Matthew and Yee, Charles and Edelman, Daniel and Vanterpool, Allan and Cheng, Anson and Gadepally, Vijay and Tiwari, Devesh},
  booktitle={2022 IEEE International Symposium on High-Performance Computer Architecture (HPCA)},
  title={AI-Enabling Workloads on Large-Scale GPU-Accelerated System: Characterization, Opportunities, and Implications},
  year={2022},
  volume={},
  number={},
  pages={1224-1237},
  doi={10.1109/HPCA53966.2022.00093}
}
```
