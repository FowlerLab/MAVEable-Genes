# CLAUDE.md — Agent Rules for MAVEable-Genes

This file provides persistent guidance for AI agents working in this repository.

## Project Overview

This repository filters ClinVar variant data by gene attributes to identify genes amenable to Multiplexed Assays of Variant Effect (MAVEs). The primary workflow is a Jupyter notebook (`ClinVar_wrangle_uniprot_gencc_merge.ipynb`) that produces a ranked list of MAVEable genes based on criteria such as essentiality, subcellular localization, protein complex membership, and known pathogenic variant associations.

## Key Files

- `ClinVar_wrangle_uniprot_gencc_merge.ipynb` — main analysis notebook; do not restructure without understanding the full pipeline
- `MAVEable_final_is_feasible.csv` — final output with feasibility flags; treat as a derived artifact
- `unique_gencc.csv` — GenCC gene-disease associations (moderate confidence and above)
- `uniprot_len_loc_2.csv` — UniProt protein length and localization data
- `subcellular_location.tsv` — subcellular localization reference
- `hap1_essential.csv.xlsx` — HAP1 cell essentiality data
- `Tcell_screen.csv` — T cell essentiality screen
- `VAMPseq-ome.csv` — VAMPseq toxicity data
- `opencell-library-metadata.csv` — OpenCell complex/interaction data
- `Rendo_2025_toxic.csv` — exogenous expression toxicity data
- `lacoste.csv` — additional reference dataset
- `instance_order.ipynb` — secondary notebook; purpose should be confirmed before editing

The large ClinVar source file (`variant_summary_*.txt.gz`) is **not committed** to this repo and must be downloaded from the NCBI FTP site.

## Language & Libraries

- **Python 3** with **Jupyter notebooks** exclusively
- Core libraries: `pandas`, `numpy`, `matplotlib`
- Do not introduce new dependencies without confirming with the user

## Coding Conventions

- Use `pandas` for all data manipulation; avoid raw Python loops over DataFrames where vectorized operations are available
- Filter steps should be explicit and commented with the biological rationale (e.g., why a ClinicalSignificance value is excluded)
- Preserve existing variable naming conventions (e.g., `ClinVar_01_25_SNV_simple`, `unique_gencc`)
- When adding new filter steps, append them in the logical order of the existing pipeline rather than inserting mid-cell
- Use `case=False, na=False` in `.str.contains()` calls for robustness
- Three-letter amino acid codes should be converted to single-letter using the existing `amino_acid_map` dictionary pattern

## Data Handling

- Treat all CSV/TSV files as read-only reference data unless the user explicitly asks to modify them
- When reading large files (e.g., ClinVar), use `low_memory=False` or explicit `dtype` arguments to avoid mixed-type warnings
- Do not hard-code absolute file paths; assume files are in the working directory
- The ClinVar assembly should always be filtered to `GRCh38`

## Output Expectations

- Final outputs are CSV files prefixed with `MAVEable_`
- Plots use `matplotlib`; maintain existing plot style unless a change is requested
- Do not overwrite existing output CSVs without explicit user confirmation

## What Not to Do

- Do not delete or rename existing CSV/TSV data files
- Do not reorder or consolidate notebook cells without understanding pipeline dependencies
- Do not commit the large ClinVar `.txt.gz` source file
- Do not add unrelated dependencies or change the Python environment
