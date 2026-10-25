# Run summary (auto-generated)

Date: 2026-10-25  |  follow-up censored at 120 months

## Signature genes used

- **Immune-evasion**: CD274, PDCD1LG2, CTLA4, LAG3, TIGIT, IDO1, CD47
- **Stemness**: ALDH1A1, CD44, PROM1, SOX2

## Sample flow

| cohort | step | n |
|---|---|---|
| TCGA-BRCA | Samples listed in the clinical file | 1084 |
| TCGA-BRCA | ... with expression data | 1082 |
| TCGA-BRCA | ... primary tumour samples only | 1082 |
| TCGA-BRCA | ... one sample per patient | 1082 |
| TCGA-BRCA | ... women only | 1070 |
| TCGA-BRCA | Final cohort (OS available: 1057) | 1070 |
| METABRIC | Samples listed in the clinical file | 2509 |
| METABRIC | ... with expression data | 1980 |
| METABRIC | ... one sample per patient | 1980 |
| METABRIC | ... women only | 1980 |
| METABRIC | Final cohort (OS available: 1979) | 1980 |

## Primary analysis (overall survival, continuous score, hazard ratio per 1 SD)

| Cohort | Module | n | Deaths | HR per SD (95% CI) | p | Adjusted HR (95% CI) | Adjusted p | PH test p (adj.) |
|---|---|---|---|---|---|---|---|---|
| TCGA-BRCA | Immune-evasion | 1057 | 138 | 0.84 (0.71-0.99) | 0.039 | 0.86 (0.71-1.04) | 0.111 | 0.979 |
| TCGA-BRCA | Stemness | 1057 | 138 | 0.78 (0.65-0.94) | 0.008 | 0.84 (0.69-1.02) | 0.075 | 0.182 |
| METABRIC | Immune-evasion | 1979 | 760 | 1.02 (0.95-1.09) | 0.548 | 0.92 (0.83-1.02) | 0.102 | 0.012 |
| METABRIC | Stemness | 1979 | 760 | 0.92 (0.86-0.99) | 0.031 | 0.98 (0.90-1.08) | 0.690 | 0.456 |

## Replication check (pre-specified rule)

| Module | TCGA-BRCA HR (95% CI) | METABRIC HR (95% CI) | Same direction | METABRIC CI excludes 1 | Replicated (rule above) |
|---|---|---|---|---|---|
| Immune-evasion | 0.84 (0.71-0.99) | 1.02 (0.95-1.09) | False | False | False |
| Stemness | 0.78 (0.65-0.94) | 0.92 (0.86-0.99) | True | True | True |

## Models where proportional hazards looks doubtful (adjusted PH test p < 0.05)

| cohort | endpoint | module | ph_test_p_adjusted |
|---|---|---|---|
| METABRIC | OS | immune_evasion | 0.0116 |
| METABRIC | RFS | immune_evasion | 0.0129 |
| METABRIC | DSS | immune_evasion | 0.0151 |

Full tables are in `survival_results.csv` and `subtype_results.csv`. Secondary endpoints are not identical across cohorts (progression-free survival in TCGA, relapse-free survival in METABRIC), so read them as supportive only.