# Contributing to XenoScan1

Thank you for your interest in XenoScan1.

## Before proposing a change

Please open an issue before making a substantial change. Describe the problem, the proposed solution and its effect on the published workflow. For bug reports, include the operating system, Python version, package version, relevant log output and a minimal reproducible example where possible. Do not upload private, sensitive or unpublished sequence data.

## Scientific and methodological changes

Changes to feature definitions, filtering thresholds, model loading, reference-manifold handling, classification thresholds or output interpretation can alter scientific results. Such changes must:

- identify the affected workflow component;
- explain the scientific or technical rationale;
- preserve the behaviour of the released version unless a change is intentional;
- include tests or a reproducible validation example;
- document changes that affect comparability with v1.0.0.

Do not describe predictions as experimentally confirmed horizontal gene transfer unless independent evidence supports that claim.

## Pull requests

Keep pull requests focused. Include a concise description, the motivation, testing performed and any change to expected outputs. Do not commit model files, reference-manifold data, private datasets, credentials or other large/generated files unless their inclusion has been explicitly agreed.

## Reproducibility

Record the XenoScan1 version, model version, reference-manifold version, Python version and dependency environment used for an analysis. Report any filtering applied to the input FASTA and retain the execution log where appropriate.

## Questions

For project questions, use the GitHub issue tracker or contact Joseph Boctor at joseph.tanyous@gmail.com.
