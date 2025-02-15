# PRIDE Affinity Proteomics (PRIDE-AP) Archive: Submission guidelines

**version**: 1.1.beta

**date**: January 09th 2025

[PRIDE Affinity Proteomics Archive: Submission guidelines]()
- [Introduction](#introduction)
- [PRIDE Archive guidelines and
        policies](#pride-archive-guidelines-and-policies)
- [Supported technologies](#supported-technologies)
- [Dataset structure and file
        requirements](#dataset-structure-and-file-requirements)
   - [Accession](#accession)
   -   [Dataset metadata](#dataset-metadata)
   -   [File requirements](#file-requirements)
-   [Ownership, privacy and release of datasets to the
 public](#ownership-privacy-and-release-of-datasets-to-the-public)
-   [Support for AP proteomics experiments in ProteomeXchange](#support-for-ap-experiments-in-proteomexchange)
-   [Conclusions](#conclusions)
    
## Introduction

PRIDE Affinity Proteomics (PRIDE-AP) is a new archive section within the Proteomics IDEntifications database (PRIDE) dedicated to advance the field of affinity proteomics (AP) by providing a centralised hub for openly accessible datasets generated using the innovative AP platforms such as Olink and SomaScan. We aim to foster collaboration, accelerate research, and promote the widespread dissemination of high-quality affinity proteomics data. This document outlines the submission guidelines for PRIDE-AP. PRIDE Archive infrastructure is used to support the submission, validation and dissemination of AP datasets. 

>[!NOTE]
>Some parts of this document will reference and link to existing guidelines in PRIDE.

## PRIDE Archive guidelines and policies

Before reading specific guidelines about affinity proteomics dataset submissions in PRIDE-AP, we recommend you to read about the following topics:

-   [Introduction to PRIDE Archive](https://www.ebi.ac.uk/pride/markdownpage/intropride).
-   [PRIDE Archive data policy](https://www.ebi.ac.uk/pride/markdownpage/datapolicy).
-   [PRIDE documentation pages: uploading, searching and visualization](https://www.ebi.ac.uk/pride/markdownpage/documentationpage).

## Supported technologies

As the moment of writing, the PRIDE-AP Archive explicitly supports Olink Explore, Olink Target, and SomaScan datasets. Future AP approaches from the same or other vendors must be formally agreed upon and explicitly supported before inclusion in PRIDE-AP.

## Dataset structure and file requirements

### Accession

All PRIDE-AP datasets get a unique accession PAD (**P**roteomics **A**ffinity **D**ataset) identifier (PAD + a six-figure integer, for example, PAD000001), which is the one that should be cited and highlighted in the scientific literature, both by the original data producers and by third parties (in case of data reuse). This accession is a unique identifier released only by the PRIDE database (please read the [section about ProteomeXchange below](#support-for-ap-experiments-in-proteomexchange)).

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

### File requirements

PRIDE-AP, like ProteomeXchange, organizes dataset files into three main categories:

- **Results (Required)**: These files include protein expression matrices and other data derived from processing the raw data. They are intended for downstream analysis, visualization, and interpretation (e.g., NPX files from Olink).
- **Raw**: These files contain the original data generated by the technology, such as raw sequencing reads or signal intensities. They are essential for data processing and analysis. In affinity proteomics, "Raw" files differ from MS technologies, where they represent MS signals. Instead, they can include various file types such as FASTQ files or non-normalized protein expression matrices.
- **Additional**: This category encompasses any supplementary data that does not fit into the previous two, such as quality control (QC) reports.

The following sections outline the specific requirements and file types for each technology.

### Olink technologies 

Olink Proteomics offers two main platforms for affinity proteomics: Olink Explore and Olink Target. Both platforms use Proximity Extension Assay (PEA) technology to quantify protein expression levels in biological samples. The data generated by these platforms is typically stored in normalized protein expression (NPX) files, which contain processed and normalized protein expression values for each sample and protein in the panel.

| Data File                     | Content                                                                       | Format | Category (Requiered) |
|-------------------------------|-------------------------------------------------------------------------------|-----|-----------|
| Normalized Protein Expression (NPX) | Normalized protein expression (NPX) values for each protein in each sample.   | CSV | Results **(Required)** |
| Raw Data File                 | Unprocessed data from the Platform. Includes raw read counts or signal intensities. | FASTQ, CSV, Parquet | Raw **(Recommended)** |
| Quality Control (QC) Report   | Summarizes quality control metrics for the experiment.                        | PDF | Additional **(Recommended)**|
| Limit of Detection (LOD) File | Provides detection limits for each protein in the panel.                      | CSV, XLSX | Additional |
| Sample Annotation             | Contains metadata about the samples used in the study.  | CSV, XLSX | Additional |
| Assay Information             | Provides detailed information about the assays used in the study. | CSV, XLSX | Additional |
| Plate Layout                  | Describes the layout of samples and controls on the assay plates. | CSV, XLSX | Additional |
| Log File                      | Records data processing steps and any errors or warnings encountered during analysis. | TXT | Additional |

> [!IMPORTANT]  
> 1. **NPX files are mandatory** for every submission to ensure datasets are comprehensive and reproducible.  
> 2. To avoid confusion between normalized NPX files and raw data files (containing unprocessed, non-normalized signals), please adhere to the following naming conventions:  
>    - **Normalized NPX files**: Use the format `file_name.npx.csv` for normalized protein expression files. This clearly identifies processed data ready for downstream analysis.  
>    - **Raw CSV files**: Name files containing unprocessed, original data (e.g., raw sequencing reads or non-normalized protein expression matrices) as `file_name.raw.csv` to distinguish foundational data required for reanalysis or alternative processing.  
>    - **Raw Ct files**: Use the format `file_name.ct.csv` for files containing unprocessed cycle threshold (Ct) data.  
>    - **Raw Parquet files**: Use the format `file_name.raw.parquet` for unprocessed data in Parquet format.
> 3. If you use the latest version of Olink Explore for the Raw data we recommend to request and provide a parquet file with unprocessed signals before normalization. By following these guidelines, submissions will adhere to a standardized structure, promoting transparency, usability, and consistency across datasets.

#### File Validation Rules

- **NPX (Normalized Protein Expression) files**: Main outputs of Olink technologies and are used for downstream protein expression analysis. These files provide processed and normalized data for consistency across samples and proteins. The following columns **MUST** be included:  

```
 - SampleID
 - OlinkID
 - UniProt
 - Assay
 - MissingFreq
 - Panel
 - LOD
 - NPX
 - Normalization
 - PlateID
```

In the current guidelines for Olink Technologies we are not validating the Raw files but we are recommending that the following information is present:

- **Raw CSV File**: Raw data files from Olink Explore contain essential experimental information, such as signal intensities, sample identifiers, and assay metadata. While the structure may vary depending on the platform version and software, the following columns **MUST** be included:  

```
- Sample Information: 
  - `Sample ID`, `Sample Plate`, `Sample Well`  
- Protein Information: 
  - `Protein ID`, `Protein Name`  
- Signal Data: 
  - `Raw Signal`, `Background Signal`, `Normalized Signal`  
- Quality Control (QC) Metrics: 
  - `LOD (Limit of Detection)`, `QC Flag`, `Replicate Consistency`  
- Control Information: 
  - `Control Type`, `Control ID`  
- Assay Metadata: 
  - `Olink Panel`, `Assay Type`, `Assay Batch`, `Assay Plate`  
- Additional Experimental Details: 
  - `Dilution Factor`, `Processing Date`, `Operator`  
```

- **Raw Parquet File**: The last Olink Explore version exports the raw data in a Parquet file format. The following columns **MUST** be included:  

```  
- Mandatory String Columns: 
  - `SampleID`, `SampleType`, `WellID`, `PlateID`, `DataAnalysisRefID`, 
  `OlinkID`, `UniProt`, `Panel`, `Block`, `Normalization`, `SampleQC`, `ExploreVersion`  
- Mandatory Integer Column: 
  - `Count`  
- Mandatory Double Columns:
  - `ExtNPX`, `NPX`  
- String Column Validation: 
  - The `SampleQC` column must have one of the following values: `NA`, `PASS`, `WARN`, or `FAIL`.  
- Data Consistency Rules:  
  - Records where `SampleQC` is `NA` or `FAIL` must have `NaN` in the `NPX` column.  
  - Records where `SampleQC` is `PASS` or `WARN` must contain a real number (not `NaN`) in the `NPX` column.  
```

- **Raw CT files**: Ct refer to the Cycle Threshold (CT) files, which contain raw data from the qPCR-based detection step in Olink assays. These files are crucial for understanding the initial fluorescence signal levels and are used in the normalization process to calculate the final Normalized Protein Expression (NPX) values. Here’s an overview of the CT files and their contents:

```
- Sample Information : 
  - Sample ID, Plate ID, Well Position
- Protein Information : 
  - Protein Name, Protein ID, Olink Panel
- Cycle Threshold (CT) Data : 
  - CT Value, Background CT
- Control Information : 
  - Control Type, Control CT Value
- Quality Control (QC) Metrics : 
  - QC Flags, Replicate Consistency
```

- **Quality Control (QC) Report**: This file provides a summary of quality control metrics for the experiment. 

### SomaScan data submissions

| Data File                     | Content                                                | Format | Category (Requiered)      |
|-------------------------------|--------------------------------------------------------|------|---------------------------|
| ADAT Results File             | Normalized protein expression in ADAT File             | ADAT | Results **(Required)**    |
| ADAT RAW File                 | Non normalized unprocess data from the platform.       | ADAT | Raw **(Recommended)**     |
| Quality Control (QC) Reports  | Summarizes quality control metrics for the experiment. | PDF  | Additional                |

SomaScan platform provides all the data in ADAT format. The ADAT format is a tab-delimited text containing information about the sample, the protein expression values and the quality control metrics. The ADAT file is the main file for the submission and it is mandatory. The ADAT RAW file is the unprocessed data from the platform and it is recommended to be submitted. The Quality Control (QC) Reports are also recommended to be submitted.

> [!IMPORTANT]
> 1. **ADAT files are mandatory** for every submission to ensure datasets are comprehensive and reproducible.
> 2. To avoid confusion between normalized ADAT files and raw data files (containing unprocessed, non-normalized signals), please adhere to the following naming conventions:
>    - **ADAT Result files**: They should be named as `file_name.adat` to identify the processed data ready for downstream analysis.
>    - **ADAT RAW files**: They should be named as `file_name.raw.adat` to distinguish foundational data required for reanalysis or alternative processing.


## Ownership, privacy and release of datasets to the public

This section is similar to [ProteomeXchange
guidelines](https://www.proteomexchange.org/docs/guidelines_px.pdf) for
MS-based proteomics submissions.

**Data Ownership**: Original submitters and Principal Investigators (PI) own the datasets submitted to PRIDE-AP resource. The submitter and PI must be explicitly identified for each dataset.

**Data availability after submission**: Submitted datasets are private (unreleased) by default, under password protection. Reviewers and editors can access data during the journal review process with the provided credentials.

**Changes to Datasets**: Changes to unreleased datasets are allowed upon request. However, after public release, changes must be justified and approved by PRIDE team.

**Public Release**: Public release of datasets is mandatory upon publication. Exceptions may be considered on a case-by-case basis. Pre-prints are not considered publications, allowing delayed release until formal acceptance.

**Withdrawal**: Unreleased datasets can be withdrawn before publication. Withdrawn entries are retained internally to prevent external access. Publicly released datasets can only be withdrawn in cases of retracted papers.

**Release Responsibility**: PX resources actively release datasets corresponding to known publications without submitters' permission. Authors should be aware that some journals may have similar release policies.

**Timely Release**: PX resources aim for prompt public release, but delays may occur due to production cycles and staff availability.

**Exceptions**: Exceptions to the public release policy may be granted in documented special cases, with a maximum 6-month extension for ongoing studies. Submitters must officially request an extension, considering journal requirements.

You can read more about ProteomeXchange guidelines [here](https://www.proteomexchange.org/docs/guidelines_px.pdf).

### Support for AP experiments in ProteomeXchange

At the time of writing, only MS-based experiments are supported by ProteomeXchange and ProteomeXchange PXD accession numbers are only released for MS-based experiments. Therefore, AP datasets stored in PRIDE will not be a part of ProteomeXchange, and the PRIDE database will independently release their accession numbers. ProteomeXchange may support AP data submissions in the future, but this is not the case at present.

## Conclusions

In conclusion, the PRIDE-AP serves as a pivotal platform dedicated to advancing the field of AP. By centralising datasets generated through technologies like Olink and SomaScan, PRIDE-AP aims to facilitate collaboration, accelerate research, and enhance the widespread dissemination of high-quality AP data. This document provides submission guidelines for datasets, emphasising the importance of adherence to PRIDE guidelines and policies.

The unique PAD accession identifier assigned to each dataset ensures proper citation in scientific literature. The document details the specific requirements for dataset structure and file formats, highlighting distinctions between SomaScan and Olink NGS-based data submissions. Additional files, experimental design, and metadata considerations are outlined to ensure comprehensive data representation. At the time of writing PRIDE-AP operates independently of ProteomeXchange for AP datasets and releases dataset accession numbers autonomously.
