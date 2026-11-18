# XenoScan1

**A machine-learning framework for alignment-free identification of candidate horizontal gene transfer (HGT) sequences using host-relative genomic discordance.**

XenoScan1 projects DNA sequences into a host-relative feature space and uses a calibrated Random Forest model to estimate HGT probability (`P_HGT`). The Windows version runs the inference workflow locally using a pre-trained model and a precomputed host reference manifold.

## Download

Download the Windows package from the **Assets** section of the [XenoScan1 v1.0.0 release](https://github.com/JosephBoctor/XenoScanHGT/releases/tag/v1.0.0).

## Package contents

| File | Description |
|---|---|
| `XenoScan1.py` | Main Python script. Contains the XenoScan1 inference workflow and starts the local run page used to control and monitor an analysis. |
| `Run_XenoScan1_Local.bat` | Windows batch file for launching XenoScan1. It checks the required Python packages, installs any that are missing, and runs the Python script. |
| `requirements_XenoScan1.txt` | Lists the Python packages required by XenoScan1. |

The trained model, host reference manifold and input FASTA file are not included in the package. You must provide these three files when running an analysis.

## Requirements

- Windows.
- Python 3, with the Windows `py` launcher available from Command Prompt.
- The three input files listed below.
- An internet connection for dependency installation when packages are missing.

The batch file checks whether the required packages are installed. If any are missing, it updates pip and installs the packages listed in `requirements_XenoScan1.txt`. Once Python and the dependencies are installed, the analysis runs locally. An internet connection is also needed if XenoScan1 has to retrieve a reference through its NCBI Entrez fallback.

## Input files

You will need:

1. **Random Forest model:** `xenoscan_rf_v1.pkl`
2. **Host reference manifold:** `XenoScan_Universal_Baseline_MASTER.parquet`
3. **Input sequences:** a FASTA file with the extension `.fasta`, `.fa` or `.fna`.

Use the pre-trained XenoScan1 model and its corresponding reference manifold. The software does not train the model or build the manifold.

### FASTA headers

Every sequence header must include an NCBI Taxonomy ID in a recognisable `taxid:` format. For example:

```text
>Sequence_1 taxid:562
ATG...
>Sequence_2 taxid:1280
ATG...
```

Sequences without a recognisable TaxID are excluded. Sequences shorter than 200 bp or with a Shannon entropy below 1.2 are also excluded.

If a TaxID is not present in the supplied reference manifold, XenoScan1 can attempt to retrieve a reference through NCBI Entrez. This requires an internet connection.

## Installation and launch

### Windows launcher

1. Download the ZIP from the v1.0.0 release and extract it.
2. Install Python 3 for Windows if it is not already installed. Make sure the Python Launcher (`py`) is included.
3. Keep `XenoScan1.py`, `Run_XenoScan1_Local.bat` and `requirements_XenoScan1.txt` in the same folder.
4. Double-click `Run_XenoScan1_Local.bat`.
5. The batch file checks the Python dependencies and installs any that are missing. This step requires an internet connection.
6. XenoScan1 starts and opens its local run page in your default browser. Leave the Command Prompt window open while the analysis is running.

### Run the Python script directly

If Python and all required packages are already installed, open Command Prompt in the package folder and run:

```bash
py XenoScan1.py
```

Running the script directly does not install missing packages. To install them manually, run:

```bash
py -m pip install -r requirements_XenoScan1.txt
```

## Running an analysis

1. Wait for the XenoScan1 page to open in your browser.
2. Click **SELECT FILES AND RUN XENOSCAN1**.
3. Windows file-selection dialogs will open. Select the Random Forest model first, the host reference manifold second, and the FASTA file third.
4. XenoScan1 will load the model and reference manifold, then process the FASTA sequences.
5. Follow the status message and execution log on the browser page.

The browser page is served by a local Python server bound to `127.0.0.1`. It is only an interface for running and monitoring XenoScan1 on your computer; it is not a remotely hosted service. The three input files are selected through Windows file dialogs, not uploaded through the browser.

When processing finishes, the page shows the output file paths. The execution log also includes the first 15 rows of the results table.

## Output files

For an input file named `sample.fasta`, XenoScan1 generates two files in the same folder as the FASTA file:

- `XenoScan1_Results_sample.csv` — sequence-level inference results.
- `XenoScan1_Analysis_sample.pdf` — a two-panel analysis figure.

The CSV contains the sequence identifier, TaxID, HGT probability, inference metrics and classification. The internal `Is_HGT` grouping column is not included in the exported CSV.

The PDF contains:

- **Panel A: 3D Inference Topology.** Sequences are plotted using standardised GC (`Z_GC`), structural anomaly (`kAI_Rel`) and translational shift (`Codon_Rel`). Colour indicates HGT probability (`P_HGT`).
- **Panel B: Probability Boundary Distributions.** Violin plots show the HGT probability distributions for the acquired-mobilome and endogenous-core groups.

The files are saved to disk beside the input FASTA. The browser page displays their paths; it does not provide a PDF preview or download buttons.

## Classification thresholds

XenoScan1 uses the following probability thresholds:

| HGT probability (`P_HGT`) | Classification |
|---:|---|
| `< 0.72` | Endogenous Core |
| `0.72` to `< 0.90` | Medium Confident HGT |
| `0.90` to `< 0.95` | Highly Confident HGT |
| `≥ 0.95` | Ultra-Confident HGT |

The acquired-mobilome grouping uses an HGT gate of `P_HGT ≥ 0.72`. These labels reflect the thresholds implemented in the model workflow. They do not, on their own, establish that horizontal transfer has occurred.

## Troubleshooting

- **The batch file cannot find Python:** Install Python 3 for Windows and make sure the Python Launcher (`py`) is installed.
- **Package installation fails:** Check your internet connection and confirm that pip is available for the Python installation used by `py`.
- **The browser page does not open:** Check the Command Prompt window for an error. Confirm that your default browser is available.
- **A file cannot be loaded:** Check that you selected the correct model (`.pkl`), reference manifold (`.parquet`) and FASTA file (`.fasta`, `.fa` or `.fna`).
- **Sequences are missing from the results:** Check the FASTA headers for TaxIDs and check that the sequences meet the length and entropy filters. The execution log reports the filtering counts and failed NCBI reference retrievals.
- **The NCBI fallback fails:** Check your internet connection and whether NCBI has a suitable reference assembly for that TaxID.
- **No output files are generated:** Check the execution log. This can happen if no sequences pass the initial filters or if inference stops because of an error.
- **You cannot find the CSV or PDF:** Both files are saved in the same folder as the input FASTA. Use the paths shown on the browser page.

## Interpreting the results

XenoScan1 reports model-derived HGT probabilities and host-relative genomic discordance metrics. These results can help identify candidate acquired sequences, but they do not establish the direction, mechanism or timing of transfer, or the biological function of a sequence. Interpret the predictions in the context of the organism, genome quality, reference coverage and other available biological evidence.

## Contact

For questions about the software, contact Joseph Boctor at [joseph.tanyous@gmail.com](mailto:joseph.tanyous@gmail.com).

## License

XenoScan1 is released under the GNU Affero General Public License v3.0 (AGPL-3.0).

Copyright 2026 Joseph Boctor.

See the `LICENSE` file for the full licence terms, or visit https://www.gnu.org/licenses/agpl-3.0.html.
