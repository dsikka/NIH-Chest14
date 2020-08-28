```mermaid
graph TD

A[Step 1: Verify dataset has not already been merged] --> B[Step 2: Are all the mandatory columns present?]
B -->|Yes| C[Keep any optional columns listed in Step 2, which are present]
B -->|No | D[Can mandatory columns be generated?]
D -->|Yes| Da[Generate mandatory columns]
D -->|No | Db[Merge cannot be completed]
Da --> C
C --> E[Step 3: Run run_ingestion_pipeline in data_ingestion.py. Remaining steps are taken care of by the script]
E --> F[Step 4: Generate sanitized mols]
F --> G[Step 5: Standardize inchis and smiles]
G --> H[Step 6: Generate standardized inchikeys]
H --> I[Step 7: Generate pChembl values, if not present]
I --> J[Step 8: Add labels and label source columns]
J --> K[Step 9: Verify all columns shown in Step 9 are present]
K --> L[Step 10: Round pChembl values to the tenths decimal place and determine overlap. Remove overlap]
L --> M[Step 11: Merge data to current frontend data. Ensure existing data is not compromised. Upload merged data]
M --> N[Step 12: For training, apply deduplication. Upload data for training]
N --> O[Step 13: Run script to determine Tanimoto similarity. Log similarities]