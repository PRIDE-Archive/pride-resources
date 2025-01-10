# PRIDE Affinity Proteomics (PRIDE-AP) Archive: Submission guidelines

**version**: 1.1.beta

**date**: January 09th 2025

- [PRIDE Affinity Proteomics Archive: Submission guidelines](https://pitchbookproperty.sharepoint.com/sites/PitchbookPropertySales/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2FPitchbookPropertySales%2FShared%20Documents%2FDevelopments%2F%2D%20CAMBRIDGE%2FCapstone%20Fields%20%2D%20CB23%207QL%2FCapstone%20Fields%20%2D%20Host%20Brochure%2Epdf&parent=%2Fsites%2FPitchbookPropertySales%2FShared%20Documents%2FDevelopments%2F%2D%20CAMBRIDGE%2FCapstone%20Fields%20%2D%20CB23%207QL&p=true&ga=1)
    -   [Introduction](#introduction)
    -   [PRIDE Archive guidelines and
        policies](#pride-archive-guidelines-and-policies)
    -   [Supported technologies](#supported-technologies)
    -   [Dataset structure and file
        requirements](#dataset-structure-and-file-requirements)
        -   [Accession](#accession)
        -   [Dataset metadata](#dataset-metadata)
        -   [File requirements](#file-requirements)
    -   [Ownership, privacy and release of datasets to the
        public](#ownership-privacy-and-release-of-datasets-to-the-public)
    -   [Support for AP proteomics experiments in ProteomeXchange](#support-for-ap-experiments-in-proteomexchange)
    -   [Conclusions](#conclusions)
    
## Introduction

PRIDE Affinity Proteomics (PRIDE-AP) is a section of the PRIDE
repository dedicated to advancing the field of affinity proteomics (AP)
by providing a centralised hub for openly accessible datasets generated
using the innovative platforms provided by companies such as Olink and
SomaLogic (the SomaScan platform). We aim to foster collaboration,
accelerate research, and promote the widespread dissemination of
high-quality affinity proteomics data. This document outlines the
submission guidelines for PRIDE-AP. PRIDE Archive
infrastructure will be used to support the submission, validation and
dissemination of AP datasets. Some parts of this document will reference
and link to existing guidelines in PRIDE.

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

At the moment of writing, Olink NGS-based submissions and SomaScan
datasets are explicitly supported by the PRIDE-AP Archive. Additional
future AP approaches provided by the same or other vendors must be agreed upon and
explicitly supported before being included in PRIDE-AP.

## Dataset structure and file requirements

### Accession

All PRIDE-AP datasets get a unique accession PAD identifier (PAD + a
six-figure integer, for example, PAD000001), which is the one that
should be cited and highlighted in the scientific literature, both by
the original data producers and by third parties (in case of data reuse). This accession
is a unique identifier released only by the PRIDE database (please read
the section about ProteomeXchange below).

### Dataset metadata

For every dataset, a minimum metadata is required similar to
ProteomeXchange dataset requirements. The following information must be
provided for each dataset: 
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

PRIDE-AP groups the files from each technology into two main 
categories: 'RAW' and 'Additional' files. The file requirements from PRIDE-AP
datasets are therefore a bit different from those for MS-based experiments included in
ProteomeXchange. [The PX
guidelines](https://www.proteomexchange.org/docs/guidelines_px.pdf)
categorize file requirements based on 'RAW' file type, peptide/protein
identification formats and search engine outputs (processed results).

### OLINK technologies : 

#### OLINK Explorer 
Olink Explore is Olink's advanced platform for high-throughput protein biomarker discovery, 
combining their proprietary Proximity Extension Assay (PEA) technology with Next-Generation 
Sequencing (NGS) as the readout method. This platform is designed to enable large-scale 
proteomics studies with high sensitivity, specificity, and scalability.

##### Olink NGS (Next Generation Sequencing)-based data submissions : Outout files and formats

|File Name   | Purpose | content  | Format |
|---|---------|----------|--------|
|  Normalized Protein Expression (NPX) File |     Contains normalized protein expression (NPX) values for each protein in each sample. Includes metadata like sample IDs, protein names, assay information, and QC flags.    |   Sample ID, Protein/Assay name, NPX values, Flags for quality control (e.g., below detection limit)       |    CSV, XLSX    |
|  Raw Data File |    Contains unprocessed sequencing data from the NGS platform. Includes raw read counts or signal intensities.     |    Sequencing reads aligned to specific DNA barcodes for proteins      |  FASTQ, CSV      |
|  Limit of Detection (LOD) File |    	Provides detection limits for each protein in the panel. Indicates which proteins are detectable in each sample.    |    Protein/Assay name, LOD values, Flags indicating detectability in each sample      |   CSV, XLSX     |
| Sample Annotation  |  	Contains metadata about the samples used in the study. Used for grouping and statistical analysis.       |  Sample ID, Grouping/condition information (e.g., treatment, time point), Other metadata (e.g., demographics)        |  CSV, XLSX      |
|  Assay Information |     Provides detailed information about the assays used in the study. Helps with biological interpretation of the results.    |   Protein name/description, Assay ID, Biological pathway or disease relevance       |  CSV, XLSX      |
|  Plate Layout |     Describes the layout of samples and controls on the assay plates. Used for troubleshooting plate-specific issues.    |  Plate ID, Well position (e.g., A1, B2), Sample/control designation        |   CSV, XLSX     |
|  Quality Control (QC) Report |     Summarizes quality control metrics for the experiment, including assay and sample performance. Includes visualizations (e.g., scatterplots, histograms) for QC evaluation.    |   Plate-level and assay-level QC metrics, Sample-level QC flags (e.g., pass/fail indicators)       |  PDF, XLSX      |
| Log File  |     Records data processing steps and any errors or warnings encountered during analysis. Useful for troubleshooting and maintaining an audit trail.   |   Time-stamped entries of processing steps, Details about normalization, QC steps, and errors       |   TXT     |

**Note** :  If your experiment is done on the latest version of OLINK Explorer then you must 
provide **.parquet file**  which was exported from OLINK Explorer which include the sample metadata, 
protein intensities, and NPX absolute quantification values.

Additionally, the parquet files have to follow certain rules to be valid

```
- The file should include the following minimum mandatory string datatype columns: SampleID, SampleType, WellID,
    PlateID, DataAnalysisRefID, OlinkID, UniProt, Panel, Block, Normalization, SampleQC, ExploreVersion
- The file should include the following minimum mandatory integer datatype column: Count
- The file should include the following minimum mandatory double datatype columns: ExtNPX, NPX
- The file has at least one record
- Possible values for string column SampleQC: NA or PASS or WARN or FAIL
- All records where the SampleQC column indicates NA or FAIL have NaN in column NPX
- All records where the SampleQC column indicates PASS or WARN have a real number, not NaN, in column NPX
```

#### OlINK Target
Olink Target Technology is a platform developed by Olink Proteomics that utilizes their
proprietary Proximity Extension Assay (PEA) technology for targeted protein biomarker analysis.
Unlike Olink Explore, which is designed for large-scale, high-throughput proteomics,
Olink Target focuses on smaller, predefined panels tailored to specific biological pathways,
disease areas, or research needs.

##### Olink Target based data submissions : Outout files and formats

| Data file | Purpose | Content | File format |
|-----------|---------|---------|-------------|
|      Raw Data: Ct Data File     |   Contains raw cycle threshold (Ct) values from qPCR      |   Cycle threshold (Ct) values for each sample and protein      |    CSV, Excel         |
|   Normalized Protein Expression (NPX) File / Abs Quant        |   Primary output file with normalized protein expression data      |     Normalized Protein Expression (NPX) values (log2 scale)    |    CSV, Excel         |
|    Quality Control (QC) Report       |     Summary of quality control results    |     	Evaluation of assay reliability, sample quality, and QC metrics    |       PDF, Excel      |
|    Plate Layout File       |     	Maps sample and control positions on plates    |    Positioning of samples and controls on 96- or 384-well plates     |      Excel, CSV       |
|        Assay Metadata File   |      Provides information on OLINK Target panels used   |   Details about the specific panels and assays used in the experiment      |    PDF, Excel         |
|      Data Normalization Report     |   Documents the normalization process      |    Explanation of the normalization steps applied to Ct data to obtain NPX     |      Text, PDF (may be part of QC report)       |
|      Summary Statistics     |    Overview of dataset and assay performance metrics     |    Key performance metrics and statistics for the assay     |    CSV, Excel         |


### SomaScan data submissions

**.adat file**: For every SomaScan submission, the .adat file containing
the 'RAW' reads, and protein quantification values without normalization MUST be
provided. In addition, normalized versions of the .adat file can be
included optionally. (We are still refining this part)


## Ownership, privacy and release of datasets to the public

This section is similar to [ProteomeXchange
guidelines](https://www.proteomexchange.org/docs/guidelines_px.pdf) for
MS-based proteomics submissions.

**Data Ownership**: Original submitters and Principal Investigators (PI)
own the datasets submitted to PRIDE-AP resource. The submitter and PI must
be explicitly identified for each dataset.

**Data availability after submission**: Submitted datasets are private (unreleased) by default, 
under password protection. Reviewers and editors can access data during the journal
review process with the provided credentials.

**Changes to Datasets**: Changes to unreleased datasets are allowed upon
request. However, after public release, changes must be justified and approved by PRIDE team.

**Public Release**: Public release of datasets is mandatory upon
publication. Exceptions may be considered on a case-by-case basis.
Pre-prints are not considered publications, allowing delayed release
until formal acceptance.

**Withdrawal**: Unreleased datasets can be withdrawn before publication.
Withdrawn entries are retained internally to prevent external access.
Publicly released datasets can only be withdrawn in cases of retracted
papers.

**Release Responsibility**: PX resources actively release datasets
corresponding to known publications without submitters' permission.
Authors should be aware that some journals may have similar release
policies.

**Timely Release**: PX resources aim for prompt public release, but
delays may occur due to production cycles and staff availability.

**Exceptions**: Exceptions to the public release policy may be granted
in documented special cases, with a maximum 6-month extension for
ongoing studies. Submitters must officially request an extension,
considering journal requirements.

You can read more about ProteomeXchange guidelines
[here](https://www.proteomexchange.org/docs/guidelines_px.pdf).

### Support for AP experiments in ProteomeXchange

At the time of writing, only MS-based experiments are supported by
ProteomeXchange and ProteomeXchange PXD accession numbers are only released for MS-based
experiments. Therefore, AP datasets stored in PRIDE will not be a part
of ProteomeXchange, and the PRIDE database will independently release
their accession numbers. ProteomeXchange may support AP data submissions
in the future, but this is not the case at present.

## Conclusions

In conclusion, the PRIDE-AP serves as a pivotal platform dedicated to
advancing the field of AP. By centralising datasets generated through
cutting-edge technologies like Olink and SomaScan, PRIDE-AP aims to
facilitate collaboration, accelerate research, and enhance the
widespread dissemination of high-quality AP data. This
document provides submission guidelines for datasets, emphasising the
importance of adherence to PRIDE guidelines and policies.

The unique PAD accession identifier assigned to each dataset ensures
proper citation in scientific literature. The document details the
specific requirements for dataset structure and file formats,
highlighting distinctions between SomaScan and Olink NGS-based
data submissions. Additional files, experimental design, and metadata
considerations are outlined to ensure comprehensive data representation.
At the time of writing PRIDE-AP operates independently of
ProteomeXchange for AP datasets and releases dataset accession numbers
autonomously.
