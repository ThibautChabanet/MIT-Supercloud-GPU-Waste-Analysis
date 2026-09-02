# GPU Idle & Phantom-Load Analysis - MIT SuperCloud

## Overview
This project analyzes GPU resource waste patterns in the MIT SuperCloud cluster through exploratory data analysis (EDA).

## Dataset & Acknowledgments
This project utilizes dcgm and scheduler data from the **MIT SuperCloud Dataset**, released as part of the following HPCA '22 publication:
> **AI-Enabling Workloads on Large-Scale GPU-Accelerated System: Characterization, Opportunities, and Implications**  
> Baolin Li, Rohin Arora, Siddharth Samsi, Tirthak Patel, et al.  
> *2022 IEEE International Symposium on High-Performance Computer Architecture (HPCA)*

## Key Findings
- **43.71%** of GPU jobs exhibit phantom-load patterns (idle/inefficient)
- **13.92%** of total energy is wasted on non-productive work
- Top 20 problem nodes account for **54%** of all waste

## Methodology
1. **Phantom-Load Detection** using 4 criteria:
   - Zero SM utilization + High power (classic idle)
   - Low SM utilization + High power (inefficient)
   - High PCIe bandwidth + Low compute (I/O phantom)
   - Long execution + Minimal work (zombie jobs)

2. **Node Analysis**: Identify which hardware exhibits waste patterns
3. **Energy Quantification**: Calculate total waste in kWh and potential savings

## Contents
- **Main.ipynb**: Complete EDA notebook with visualizations and analysis

## Results
- Total analysis: 96,893 GPU job records
- Phantom-load jobs: 42,355 (43.71%)
- Energy wasted: 370 kWh (13.92%)
- Opportunity: Fix top 20 nodes to save 199 kWh

## Next Steps
- Cost & CO2 impact calculations
- Job duration breakdown analysis
- Partition-level analysis
- Scheduling improvements recommendations

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

