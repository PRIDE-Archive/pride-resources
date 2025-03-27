# PRIDE Affinity Proteomics (PRIDE-AP) Archive: Submission guidelines

**version**: 1.0.0

**date**: March 27th 2025

[PRIDE Affinity Proteomics Archive: Submission guidelines]()
- [Introduction](#introduction)
- [PRIDE Archive guidelines and
  policies](#pride-archive-guidelines-and-policies)
- [Supported technologies](#supported-technologies)
- [Dataset structure and file
  requirements](#dataset-structure-and-file-requirements)
    - [Accession](#accession)
    -   [Dataset metadata](#dataset-metadata)
    -   [Olink and SomaScan file requirements](#olink-and-somascan-file-requirements)
-   [Ownership, privacy and release of datasets to the
    public](#ownership-privacy-and-release-of-datasets-to-the-public)
-   [Support for AP proteomics experiments in ProteomeXchange](#support-for-ap-experiments-in-proteomexchange)
-   [Conclusions](#conclusions)

## Introduction

PRIDE Affinity Proteomics (PRIDE-AP) is a new Archive within PRIDE repository dedicated to advancing the field of affinity proteomics (AP)
by providing a centralised hub for openly accessible datasets generated
using the innovative AP platforms such as Olink and SomaLogic (the SomaScan platform). 

We aim to foster collaboration, accelerate research, and promote the widespread dissemination of high-quality affinity proteomics data. This document outlines the submission guidelines for PRIDE-AP. PRIDE Archive infrastructure will be used to support the submission, validation and dissemination of AP datasets. Some parts of this document will reference and link to existing guidelines in PRIDE.

## PRIDE Archive guidelines and policies

Before reading specific guidelines about affinity proteomics dataset
submissions in PRIDE-AP, you may read about the following topics:

-   [Introduction to PRIDE
    Archive](https://www.ebi.ac.uk/pride/markdownpage/intropride).
-   [PRIDE Archive data
    policy](https://www.ebi.ac.uk/pride/markdownpage/datapolicy).
-   [PRIDE documentation pages: uploading, searching and
    visualization](https://www.ebi.ac.uk/pride/markdownpage/documentationpage).

## Supported technologies

At the moment of writing, the following methods are supported: 
 - Olink Explore
 - Olink Target
 - SomaScan datasets

> [!NOTE]
> Any future AP approaches from the same or other vendors must be reviewed, agreed upon, and explicitly supported before being included in PRIDE-AP.

## Dataset structure and file requirements

### Accession

All PRIDE-AP datasets get a unique accession `PAD` (**P**roteomics **A**ffinity **D**ataset) identifier (PAD + a six-figure integer, for example, PAD000001), which is the one that should be cited and highlighted in the scientific literature, both by the original data producers and by third parties (in case of data reuse). This accession is a unique identifier released only by the PRIDE database (please read the section about [ProteomeXchange below](#support-for-ap-experiments-in-proteomexchange)).

### Dataset metadata

For every dataset, a minimum metadata is required similar to ProteomeXchange dataset requirements. The following information must be provided for each dataset:
- Title (free-text)
- Description (free-text)
- Sample analysis protocol (free-text)
- Data analysis protocol (free-text)
- Data submitter (name, affiliation, e-mail address)
- Principal Investigator (name, affiliation, e-mail address)
- Species (ontology or controlled vocabulary (CV) term)
- Tissue (ontology/CV term)
- List using ontology/CV terms of Instrument, Species, Tissue, Software
- Quantification method (ontology/ CV term).

## Olink and SomaScan file requirements

### Olink technologies:

The following sections outline the specific requirements and file types for each technology.

#### Olink NGS-based data submissions

| Data File                           | Content                                                                                                                                                                    | Format              | Category (Required)    |
|-------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------|-------------------------|
| Normalized Protein Expression (NPX)* | Normalized protein expression (NPX) values for each protein in each sample.                                                                                                | CSV, XLSX           | Results (Required)     |
| Raw Data File**                     | Contains unprocessed sequencing data from the NGS platform. Includes raw read counts or signal intensities.                                                                | FASTQ, CSV, Parquet | Raw (Recommended)      |
| Limit of Detection (LOD) File       | Provides detection limits for each protein in the panel. Indicates which proteins are detectable in each sample.                                                          | CSV, XLSX           | Additional              |
| Sample Annotation                   | Contains metadata about the samples used in the study. Used for grouping and statistical analysis.                                                                        | CSV, XLSX           | Additional              |
| Assay Information                   | Provides detailed information about the assays used in the study. Helps with biological interpretation of the results.                                                     | CSV, XLSX           | Additional              |
| Plate Layout                        | Describes the layout of samples and controls on the assay plates. Used for troubleshooting plate-specific issues.                                                          | CSV, XLSX           | Additional              |
| Quality Control (QC) Report         | Summarizes quality control metrics for the experiment, including assay and sample performance. Includes visualizations (e.g., scatterplots, histograms) for QC evaluation. | PDF, XLSX           | Additional              |
| Log File                            | Records data processing steps and any errors or warnings encountered during analysis. Useful for troubleshooting and maintaining an audit trail.                           | TXT                 | Additional              |

> [!NOTE]
> NPX* (Result file) example name: {file name}.npx.cs
> 
> RAW** (RAW file) example name: {file name}.raw.csv

##### npx.csv/npx.parquet validations

If your experiment is done on Olink Explore HT then you must provide a `.parquet file` which was exported from Olink Explore HT which includes the sample metadata, protein intensities, and NPX absolute quantification values. If the experiment is done on any other version of Olink Explore, then you can provide a `.csv file`. 

**NPX file validation**:

The NPX (Normalized Protein Expression) data files from Olink Explore are the primary output used for downstream analysis of protein expression. These files provide processed and normalized data, ensuring comparability across samples and proteins.

Here’s an overview of the columns commonly found in NPX files:

```

- Sample Information : Sample ID, Sample Plate, Sample Well, Group/Condition
- Protein Iformation : Protein ID, Protein Name, Olink Panel, LOD
- NPX Values : NPX Value, Below LOD
- Quality Control (QC) Metrics : QC Flags, Plate Control
- Control Information : Control Type, Control NPX
- Additional Metadata (Optional) : Experiment Date, Batch ID, Operator, Sample Metadata
```

**Parquet** file validation:

```
- The file should include the following minimum mandatory string datatype columns: SampleID, SampleType, WellID, PlateID, DataAnalysisRefID, OlinkID, UniProt, Panel, Block, Normalization, SampleQC, ExploreVersion
- The file should include the following minimum mandatory column: Count (Integer datatype); ExtNPX (double datatype), NPX (double datatype)
- The file has at least one record
- Possible values for string column SampleQC: NA or PASS or WARN or FAIL
- All records where the SampleQC column indicates NA or FAIL have NaN in column NPX
- All records where the SampleQC column indicates PASS or WARN have a real number, not NaN, in column NPX
```

#### Olink Target series

Olink Target Technology is a platform developed by Olink Proteomics that utilizes their proprietary Proximity Extension Assay (PEA) technology for targeted protein biomarker analysis. Unlike Olink Explore, which is designed for large-scale, high-throughput proteomics, Olink Target focuses on smaller, predefined panels tailored to specific biological pathways, disease areas, or research needs.

##### Olink Target data submissions

| Data file                                             | Content                                                     | File format                                         | Category (Required)    |
|-------------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------|-------------------------|
| Normalized Protein Expression (NPX) File / Abs Quant* | Primary output file with normalized protein expression data | CSV, Excel                                         | Results (Required)     |
| Ct Data File**                                        | Contains raw cycle threshold (Ct) values from qPCR          | CSV, Excel                                         | Raw (Recommended)      |
| Quality Control (QC) Report                           | Summary of quality control results                          | PDF, Excel                                         | Additional              |
| Plate Layout File                                     | Maps sample and control positions on plates                 | Excel, CSV                                         | Additional              |
| Assay Metadata File                                   | Provides information on Olink Target panels used            | PDF, Excel                                         | Additional              |
| Data Normalization Report                             | Documents the normalization process                         | Text, PDF (may be part of QC report)                 | Additional              |
| Summary Statistics                                    | Overview of dataset and assay performance metrics           | CSV, Excel                                         | Additional              |

> [!NOTE]
> NPX* (Result file) example name: {file name}.npx.csv
>
> RAW** (RAW file) example name: {file name}.ct.csv

##### Raw File and Result validations for Olink Explore

**Ct Data File validation**: CT files refer to the Cycle Threshold (CT) files, which contain raw data from the qPCR-based detection step in Olink assays. These files are crucial for understanding the initial fluorescence signal levels and are used in the normalization process to calculate the final Normalized Protein Expression (NPX) values.

Here’s an overview of the CT files and their contents:

```
- Sample Information : Sample ID, Plate ID, Well Position
- Protein Information : Protein Name, Protein ID, Olink Panel
- Cycle Threshold (CT) Data : CT Value, Background CT
- Control Information : Control Type, Control CT Value
- Quality Control (QC) Metrics : QC Flags, Replicate Consistency
```

**NPX file validation**: From Olink Target is the key output file for analyzing protein expression levels after raw data has been processed and normalized. The NPX values are calculated to provide log2-transformed, normalized expression levels for proteins in each sample. The NPX file contains several columns, each providing essential data for downstream analysis.
Here are the typical columns found in an NPX data file from Olink Target:
```
- Sample Information : Sample ID, Sample Plate, Sample Well, Group/Condition
- Protein Information : Protein Name, Protein ID, Olink Panel
- NPX Data : NPX Value, Below LOD (Limit of Detection), LOD (Limit of Detection)
- Quality Control (QC) Metrics - QC Flags, Replicate Consistency
- Control Information : Control Type, Control NPX
- Metadata (Optional) : Batch Id, Operator, Assay Plate, Experiment date
```

### SomaScan based submissions

SomaScan technology can measure thousands of proteins in biological samples, such as blood. It works by using a unique method called SOMAscan, which allows for the detailed analysis of proteins at a large scale.

| Output Format          | Description                                                                                                   | File Extension |
|------------------------|---------------------------------------------------------------------------------------------------------------|----------------|
| *.adat                 | Primary output file containing protein expression data, sample metadata, and quality control (QC) information | .adat          |
| CSV/TSV                | Processed data exported from .adat files for easier handling in Excel, Python, or R                           | .csv / .tsv    |
| Excel (XLSX)           | Sometimes used for reporting normalized RFU values, QC metrics, and metadata                                  | .xlsx          |
| PDF Reports            | Quality control summary reports with visualizations and QC flags                                              | .pdf           |
| R Data Files (RDS/RDA) | When using R for analysis, .adat files may be converted to R-compatible formats                               | .rds / .rda    |

> [!NOTE]
> Example for naming your .adat file: {file name}.adat

**.adat file**: In a SomaScan experiment, a .adat file is the primary data output format containing the processed results. It is a structured file format developed by SomaLogic to store the assay data, including protein measurements, sample metadata, and quality control (QC) information.

| Section            | Description                                                                                                                                             |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Header Section     | Contains metadata about the experiment, including the SomaScan version, assay date, normalization details, and quality control (QC) thresholds          |
| Annotation Section | Describes the protein targets and SOMAmer reagents used, including Target Name (protein name), SOMAmer ID, UniProt ID, and Dilution Group               |
| Data Section       | The main numerical data table with Sample IDs, Relative Fluorescence Units (RFU) for each protein, and QC Flags indicating potential measurement issues |

> [!NOTE] 
> The .adat file without normalization MUST be provided. In addition, normalized versions of the .adat file can be included optionally but should be mentioned specifically in the file name.

## Ownership, privacy and release of datasets to the public

This section is similar to [ProteomeXchange guidelines](https://www.proteomexchange.org/docs/guidelines_px.pdf) for MS-based proteomics submissions.

**Data Ownership**: Original submitters and Principal Investigators (PI) own the datasets submitted to PRIDE-AP resource. The submitter and PI must be explicitly identified for each dataset.

**Data availability after submission**: Submitted datasets are private (unreleased) by default, under password protection. Reviewers and editors can access data during the journal review process with the provided credentials.

**Changes to Datasets**: Changes to unreleased datasets are allowed upon request. However, after public release, changes must be justified and approved by PRIDE team.

**Public Release**: Public release of datasets is mandatory upon publication. Exceptions may be considered on a case-by-case basis. Pre-prints are not considered publications, allowing delayed release until formal acceptance.

**Withdrawal**: Unreleased datasets can be withdrawn before publication. Withdrawn entries are retained internally to prevent external access. Publicly released datasets can only be withdrawn in cases of retracted papers.

**Release Responsibility**: PX resources actively release datasets corresponding to known publications without submitters' permission. Authors should be aware that some journals may have similar release policies.

**Timely Release**: PX resources aim for prompt public release, but delays may occur due to production cycles and staff availability.

**Exceptions**: Exceptions to the public release policy may be granted in documented special cases, with a maximum 6-month extension for ongoing studies. Submitters must officially request an extension, considering journal requirements.

You can read more about ProteomeXchange guidelines [here](proteomexchange-guidelines.md).

### Support for AP experiments in ProteomeXchange

Please note that at the time of writing this document, only MS-based experiments are supported by ProteomeXchange, and ProteomeXchange PXD accession numbers are only released for MS-based experiments.  Therefore, PRIDE-AP datasets will not be part of ProteomeXchange, and PRIDE will independently release accession numbers.  This situation may change in the future.  Please check the ProteomeXchange website for the most up-to-date information.

## Conclusions

In conclusion, the PRIDE-AP serves as a pivotal platform dedicated to advancing the field of AP. By centralising datasets generated through cutting-edge technologies like Olink and SomaScan, PRIDE-AP aims to facilitate collaboration, accelerate research, and enhance the widespread dissemination of high-quality AP data. This document provides submission guidelines for datasets, emphasising the importance of adherence to PRIDE guidelines and policies.

The unique PAD accession identifier assigned to each dataset ensures proper citation in scientific literature. The document details the specific requirements for dataset structure and file formats, highlighting distinctions between SomaScan and Olink NGS-based data submissions. Additional files, experimental design, and metadata considerations are outlined to ensure comprehensive data representation. At the time of writing PRIDE-AP operates independently of ProteomeXchange for AP datasets and releases dataset accession numbers autonomously.