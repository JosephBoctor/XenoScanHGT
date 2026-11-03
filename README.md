# XenoScan1

**A machine-learning framework for alignment-free identification of candidate horizontal gene transfer (HGT) sequences through host-relative genomic discordance.**

XenoScan1 projects input DNA sequences into a host-relative feature space and uses a calibrated Random Forest model to estimate HGT probability (`P_HGT`). The standalone Windows edition provides a local browser interface for running inference with a pre-trained model and a precomputed host reference manifold.

## Download

Download the standalone Windows package from the **Assets** section of the [XenoScan1 v1.0.0 release](https://github.com/JosephBoctor/XenoScanHGT/releases/tag/v1.0.0).

## Contents

The standalone package contains:

| File | Purpose |
|---|---|
| `XenoScan1.py` | Main application. Starts the local browser interface and runs the inference workflow. |
| `Run_XenoScan1_Offline.bat` | Windows launcher. Checks for the required Python packages, installs them if needed, and starts XenoScan1. |
| `requirements_XenoScan1.txt` | Python package dependencies used by the launcher. |

The trained model, reference manifold and input FASTA file are **not included** in the application package. They must be supplied by the user.

## Requirements

- Windows.
- Python 3 installed with the Windows `py` launcher available from Command Prompt.
- The three input files listed below.
- Internet access for first-time dependency installation if the required Python packages are not already installed.

The launcher installs the packages listed in `requirements_XenoScan1.txt` when they are missing. The core analysis can run locally once Python, the dependencies and required input files are available. Internet access is required if XenoScan1 needs to retrieve a host reference through its NCBI Entrez fallback.

## Required input files

Select the following files in the browser interface:

1. **Random Forest model:** `xenoscan_rf_v1.pkl`
2. **Host reference manifold:** `XenoScan_Universal_Baseline_MASTER.parquet`
3. **Target sequences:** a FASTA file with the extension `.fasta`, `.fa` or `.fna`.

The model and manifold must be the compatible, pre-trained XenoScan1 assets. The application does not train a new model or generate the reference manifold.

### FASTA header requirement

Each sequence header must contain an NCBI Taxonomy ID in a recognisable `taxid:` format. For example:

```text
>Sequence_1 taxid:562
ATG...
>Sequence_2 taxid:1280
ATG...
```

Sequences without a recognisable TaxID are counted as missing TaxID and are not processed. Sequences shorter than 200 bp or failing the Shannon entropy filter are also excluded. If a TaxID is not represented in the supplied reference manifold, the inference engine can attempt to retrieve a de novo reference through NCBI Entrez; that fallback requires an internet connection.

## Installation and launch

### Option 1: Windows launcher (recommended)

1. Download and extract the complete XenoScan1 package into a folder.
2. Install Python 3 for Windows if it is not already installed. Ensure the Python Launcher (`py`) is included.
3. Keep `XenoScan1.py`, `Run_XenoScan1_Offline.bat` and `requirements_XenoScan1.txt` together in the same folder.
4. Double-click `Run_XenoScan1_Offline.bat`.
5. The launcher checks the required packages and installs missing dependencies. An internet connection is needed if installation is required.
6. XenoScan1 opens in the default web browser. Keep the launcher window open while the application is running.

### Option 2: Run the Python file directly

If Python and all required packages are already installed, open Command Prompt in the application folder and run:

```bash
py XenoScan1.py
```

Direct execution does not install missing dependencies. If a dependency is missing, install the requirements manually:

```bash
py -m pip install -r requirements_XenoScan1.txt
```

## Running an analysis

1. In the browser interface, select the `.pkl` Random Forest model.
2. Select the `.parquet` host reference manifold.
3. Select the target FASTA file.
4. Click **RUN XENOSCAN1**.
5. Follow the execution status and log in the browser interface.
6. When inference finishes, the interface presents the PDF preview, the first 15 rows of the results table, and download links for the output files. The application also attempts to trigger the CSV and PDF downloads automatically; browser download settings may affect automatic downloads.

The application runs on the local computer. The browser interface communicates with a local server bound to `127.0.0.1`; it is not a hosted web service.

## Output files

For an input FASTA file named `sample.fasta`, XenoScan1 generates:

- `XenoScan1_Results_sample.csv` — sequence-level inference results.
- `XenoScan1_Analysis_sample.pdf` — a two-panel analysis figure.

The CSV contains the sequence identifier, TaxID, HGT probability and associated inference metrics/classification. The export omits the internal `Is_HGT` grouping column. The PDF contains:

- **Panel A: 3D Inference Topology**, displaying sequence positions across standardised GC (`Z_GC`), structural anomaly (`kAI_Rel`) and translational shift (`Codon_Rel`), with colour representing HGT probability (`P_HGT`).
- **Panel B: Probability Boundary Distributions**, showing the probability distributions for the acquired-mobilome and endogenous-core groups.

The output files are made available through the browser interface for download.

## Classification thresholds

The current inference workflow uses the following probability boundaries:

| HGT probability (`P_HGT`) | Classification |
|---:|---|
| `< 0.72` | Endogenous Core |
| `0.72` to `< 0.90` | Medium Confident HGT |
| `0.90` to `< 0.95` | Highly Confident HGT |
| `≥ 0.95` | Ultra-Confident HGT |

The HGT gate used for the acquired-mobilome grouping is `P_HGT ≥ 0.72`. These are the thresholds implemented in this release; they should not be interpreted as independent experimental confirmation of horizontal transfer.

## Troubleshooting

- **The launcher reports that Python was not found:** Install Python 3 for Windows and ensure the Python Launcher (`py`) is installed.
- **Dependency installation fails:** Check the internet connection and that `pip` is available for the Python installation used by `py`.
- **The browser interface does not open:** Check the launcher window for an error. You can open the local URL printed by the application manually if necessary.
- **The application reports an invalid input:** Confirm that the model is a `.pkl` file, the reference manifold is a `.parquet` file and the sequence input is `.fasta`, `.fa` or `.fna`.
- **Sequences are filtered or missing from the results:** Check that FASTA headers contain TaxIDs and that sequences meet the minimum length and entropy filters. Review the execution log for filtering counts and NCBI baseline retrieval failures.
- **The NCBI fallback fails:** Check internet connectivity and whether NCBI can return a suitable reference assembly for the supplied TaxID.
- **The CSV or PDF does not download automatically:** Use the download links displayed in the results section. Browser settings may block automatic multiple-file downloads.

## Scientific scope and interpretation

XenoScan1 reports model-derived HGT probabilities and host-relative genomic dissonance metrics. Predictions are computational evidence for candidate acquired sequences, not proof of transfer direction, mechanism, timing or biological function. Results should be interpreted in the context of the organism, genome quality, reference coverage and independent biological evidence.

## Citation and contact


For questions about the implementation, contact **Joseph Boctor**: [joseph.tanyous@gmail.com](mailto:joseph.tanyous@gmail.com).

## License

XenoScan1 is released under the Apache License, Version 2.0.

Copyright 2026 Joseph Boctor.

You may use, reproduce, modify and distribute this software under the terms of the Apache License 2.0. See the `LICENSE` file for the full licence terms.

https://www.apache.org/licenses/LICENSE-2.0
