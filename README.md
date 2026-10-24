# Do immune-evasion and stemness gene scores track survival in breast cancer?

A survival analysis in TCGA-BRCA, replicated in METABRIC.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dochieng014-code/breast-cancer-immune-stemness-survival/blob/main/breast_cancer_signature_survival.ipynb)

## Why I did this

My review on immune editing of stem cell fate in cancer (Journal of Cancer and Tumor International, 2026) argued that tumours which escape immune control and tumours with stem-like features are closely linked. I wanted to see whether that idea leaves a measurable trace in real patient data, using only public cohorts and a signature I fixed before looking at any outcome.

I come from a wet-lab biochemistry background, so this project is also my way of showing that I can take a biological question all the way to a reproducible statistical answer.

## The question

1. Do tumours with a high **immune-evasion** score have different overall survival from tumours with a low score?
2. Do tumours with a high **stemness** score?
3. Is the pattern seen in TCGA-BRCA reproduced in METABRIC, an independent cohort measured on a different platform?

## Data

Both cohorts are public and de-identified, downloaded from the cBioPortal data hub. Nothing in this repository comes from my hospital internship or from any private source.

| Cohort | cBioPortal study | Expression platform | Role |
|---|---|---|---|
| TCGA-BRCA | `brca_tcga_pan_can_atlas_2018` | RNA-seq (RSEM) | Discovery |
| METABRIC | `brca_metabric` | Illumina microarray | Validation |

Exact sample counts at every filtering step are written to `results/sample_flow.csv` when the notebook runs.

## The signature

I chose the genes from biology and fixed them in advance. They are not weighted or fitted to survival, so there is no training step that could overfit.

| Module | Genes |
|---|---|
| Immune evasion | CD274, PDCD1LG2, CTLA4, LAG3, TIGIT, IDO1, CD47 |
| Stemness | ALDH1A1, CD44, PROM1, SOX2 |

Only genes present in both cohorts are used, and the notebook stops with a clear message instead of quietly changing the signature if a module loses too many genes.

One caution I built into the design: several checkpoint genes are mostly expressed by immune cells inside the tumour. A high immune-evasion score can therefore mean a heavily inflamed tumour, which in some subtypes goes with better outcome. For this reason the two modules are analysed separately and I did not assume a direction in advance.

## Methods in brief

1. Keep primary tumours, one sample per patient, women only. Follow-up is censored at 10 years in both cohorts.
2. Put expression on a log2 scale, convert each gene to a z-score **within each cohort**, and average the genes in each module. This removes the platform difference between RNA-seq and microarray without using any outcome information.
3. **Primary analysis:** Cox proportional-hazards model for overall survival, hazard ratio per 1 SD of the score.
4. **Adjusted model:** age, stage and molecular subtype, with a proportional-hazards check and Harrell's C-index.
5. **Kaplan-Meier curves** (median split) with log-rank test, for display.
6. **Replication rule, written before running:** a module counts as replicated if the METABRIC effect has the same direction as TCGA-BRCA and its 95% confidence interval excludes 1.
7. Exploratory only: the combined score, secondary endpoints (progression-free survival in TCGA, relapse-free and disease-specific survival in METABRIC), and subtype-specific models. Benjamini-Hochberg adjusted p-values are reported across all tests.

Tools: Python (pandas, lifelines, matplotlib) in Google Colab.

## Results

> **Status: not yet filled in.** Run the notebook with `USE_DEMO_DATA = False`, then paste the numbers from `results/summary.md` and the figures below. Only report what the run produces.

<!--
Paste here, in your own words:
- sample sizes and number of deaths per cohort (from results/sample_flow.csv)
- the primary hazard ratios with 95% CI and p-values, unadjusted and adjusted
- whether each module met the replication rule
- the figures: figures/km_OS_*.png, figures/forest_all_results.png
-->

## Limitations

- Both cohorts are retrospective and mostly from patients in Europe and North America. These results say nothing about whether the scores behave the same way in African patients; answering that needs African cohorts, and public data of that kind are scarce.
- The signature is small and was chosen from biological reasoning, not optimised. A different reasonable gene list could give a different answer.
- The immune-evasion score cannot separate "the tumour is evading the immune system" from "the tumour is heavily infiltrated by immune cells".
- An association with survival does not show that these genes cause the outcome or that they are drug targets.
- TCGA has short median follow-up and few deaths, so its estimates are imprecise. METABRIC has longer follow-up but different treatment eras and a different platform.
- Overall survival in METABRIC includes deaths from other causes, so I also report disease-specific survival there.

## Run it yourself

1. Click the Colab badge at the top of this page (the repository must be public for the link to work).
2. *Runtime → Run all*. The first run downloads the two cohorts (a few hundred MB) and takes roughly 10-20 minutes.
3. Outputs appear in `results/` (tables, `summary.md`, `run_info.json` with software versions and file checksums) and `figures/`. The last cell zips them for download.

To test the code without downloading anything, set `USE_DEMO_DATA = True`. That uses simulated data, writes to `demo_outputs/` and stamps every figure "SIMULATED DEMO DATA". Those outputs must never be quoted as findings.

Files:

```
breast_cancer_signature_survival.ipynb   the whole analysis, start to finish
requirements.txt                          packages (Colab already has most)
results/   figures/                       created when the notebook runs
```

## How I used AI tools

I used Claude to help design the analysis structure and write and debug the code. I reviewed the method, ran the notebook myself, and I am responsible for the interpretation. The code was tested on simulated data with a known built-in effect to confirm that hazard-ratio directions come out correctly before it was pointed at real cohorts.

## References

- Curtis C, Shah SP, Chin SF, et al. The genomic and transcriptomic architecture of 2,000 breast tumours reveals novel subgroups. *Nature* 2012;486(7403):346-352. (METABRIC)
- Cancer Genome Atlas Network. Comprehensive molecular portraits of human breast tumours. *Nature* 2012;490(7418):61-70. doi:10.1038/nature11412
- Sammut SJ, Crispin-Ortuzar M, Chin SF, et al. Multi-omic machine learning predictor of breast cancer therapy response. *Nature* 2022;601(7894):623-629. doi:10.1038/s41586-021-04278-5 (an example of combining transcriptomic immune features with clinical data to predict outcome)
- Baird RD, Caldas C. Genetic heterogeneity in breast cancer: the road to personalized medicine? *BMC Medicine* 2013;11:151. (why subtype matters when interpreting any breast cancer signature)
- Cerami E, Gao J, Dogrusoz U, et al. The cBioPortal for Cancer Genomics. *Cancer Discovery* 2012;2(5):401-404.
- Davidson-Pilon C. lifelines: survival analysis in Python. *Journal of Open Source Software* 2019;4(40):1317.
- Cox DR. Regression models and life-tables. *J R Stat Soc B* 1972;34:187-220.
- Benjamini Y, Hochberg Y. Controlling the false discovery rate. *J R Stat Soc B* 1995;57:289-300.

## Author

Dickens Agie, Biochemist, Nairobi, Kenya
dickoc038@gmail.com

## Licence

Code: MIT. Please cite this repository if you reuse it. The underlying data belong to their original studies; follow cBioPortal's terms of use.
