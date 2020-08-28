```mermaid
graph TD

Step # refer to `data_merge_steps.md`
Functions in `data_ingestion.py`

title[<u>Data Ingestion Pipeline</u>]
title -->A 
A[Verify dataset has not already been merged] --> B[Are all the mandatory columns present?]
B -->|Yes| C[Keep any optional columns listed in Step 2, which are present]
B -->|No | D[Can mandatory columns be generated?]
D -->|Yes| Da[Generate mandatory columns]
D -->|No | Db[Merge cannot be completed]
Da --> C
C --> E[Run run_ingestion_pipeline in data_ingestion.py. Remaining steps are taken care of by the script]
E --> F[Generate sanitized mols]

F --> G[Standardize inchis and smiles]
G --> H[Generate standardized inchikeys]
H --> I[Generate pChembl values, if not present]
I --> J[Add labels and label source columns]
J --> K[Verify all columns shown in Step 9 are present]
K --> L[Round pChembl values to the tenths decimal place and determine overlap. Remove overlap]
L --> M[Merge data to current frontend data. Ensure existing data is not compromised. Upload merged data]
M --> N[For training, apply deduplication. Upload data for training]
N --> O[Run script to determine Tanimoto similarity. Log similarities]