# Data Ingestion Flowchart

#### Step # refer to [`data_merge_steps.md`](./data_merge_steps.md)
#### Functions can be found in [`data_ingestion.py`](./data_ingestion.py)

```mermaid
graph TD

title[DATA INGESTION PIPELINE]
title -->A 
A["Verify dataset has not already been smerged (Step 1)"] --> B["Are all the mandatory columns present? (Step 2)"]
B -->|Yes| C[Keep any optional columns listed in Step 2, which are present]
B -->|No | D[Can mandatory columns be generated?]
D -->|Yes| Da[Generate mandatory columns]
D -->|No | Db[Merge cannot be completed]
Da --> C
C --> E[Run run_ingestion_pipeline in data_ingestion.py. Remaining steps are taken care of by the script]
E --> F["Generate sanitized mols (Step 4, `generate_sanitized_mols`)"]
F --> G["Standardize inchis and smiles (Step 5, `standardize_inchis_smiles`)"]
G --> H["Generate standardized inchikeys (Step 6, `get_standardized_inchikeys`)"]
H --> I["Generate pChembl values, if not present (Step 7, `generate_pchembl`)"]
I --> J["Add labels and label source columns (Step 8, `add_label`, `add_label_source`)"]
J --> K[Verify all columns shown in Step 9 are present, `check_all_columns_present`]
K --> L["Round pChembl values to the tenths decimal place and determine overlap. Remove overlap (Step 10, `remove_overlap`)"]
L --> M["Merge data to current frontend data. Ensure existing data is not compromised. Upload merged data (Step 11, `merge_with_frontend`)"]
M --> N["For training, apply deduplication. Upload data for training (Step 12, `prepare_for_training`)"]
N --> O["Run script to determine Tanimoto similarity. Log similarities (Step 13)"]