# GPU Idle & Phantom-Load Analysis - MIT SuperCloud

## Overview
This project analyzes GPU resource waste patterns in the MIT SuperCloud cluster through exploratory data analysis (EDA).

## Data
Analysis uses internal GPU metrics (DCGM) and scheduler metadata.
*Data not included in repository per institutional policy.*

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
