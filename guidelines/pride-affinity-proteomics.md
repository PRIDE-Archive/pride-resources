# PRIDE Affinity Proteomics Archive: Submission guidelines
**version**: 1.0.beta

**date**: 18 January 2024. 

## Introduction 

The PRIDE Affinity Proteomics Archive (PRIDE-AP) is a repository dedicated to advancing the field of affinity proteomics by providing a centralised hub for datasets generated using innovative technologies such as Olink and SomaScan. Our mission is to foster collaboration, accelerate research, and promote the widespread dissemination of high-quality affinity proteomics data. This document outlines the submission guidelines and repository for the PRIDE-AP Archive. PRIDE infrastructure will be used to support the submission, validation and dissemination of datasets. Some parts of this document will reference and link to existing guidelines in PRIDE.

## PRIDE Archive guidelines and policies

Before reading specific guidelines about affinity proteomics datasets submission in PRIDE-AP, you may read about the following topics: 

- [Introduction to PRIDE Archive](https://www.ebi.ac.uk/pride/markdownpage/intropride).
- [PRIDE Archive data policy](https://www.ebi.ac.uk/pride/markdownpage/datapolicy). 
- [PRIDE documentation pages: uploading, searching and visualization](https://www.ebi.ac.uk/pride/markdownpage/documentationpage).
  
## Supported technologies

From Jan 2024, only Olink NGS-based submissions and SomaScan datasets will be supported by the PRIDE-AP Archive. Future technologies and analytical methods must be agreed upon with the corresponding laboratories or companies before being included.

## Dataset structure and file requirements

### Accession

All PRIDE-AP datasets get a unique accession PAD identifier (PAD + a six-figure integer, for example, PAD000001), which is the one that should be cited and highlighted in the scientific literature, both by the original data producers and by other third parties. This accession is a unique identifier released only by the PRIDE database (please read the section about ProteomeXchange). 

### Dataset metadata

For every dataset, a minimum metadata is required similar to ProteomeXchange dataset requirements. The following information must be provided for each dataset: 
- Title
- Description
- Sample protocol
- Data protocol
- List using ontology terms of Instrument, Species, Tissue, Biospecimens, Software
- Quantification method. 

### File requirements

PRIDE affinity proteomics experiments file requirements are different from those in MS-based experiments in ProteomeXchange. The PX guidelines categorize file requirements based on RAW file type, peptide/protein identification formats, and search engine outputs. Similarly, PRIDE-AP datasets group the files from each technology into two main categories: RAW and Additional files. 

#### RAW files

Similar to MS-based submissions PRIDE-AP datasets MUST provide a RAW file. This category suggests that the data is in its raw, unprocessed form from the platform or instrument. In MS-based proteomics, RAW files are mainly mass spectrometry files containing mass spectra. In affinity proteomics datasets, each technology has defined different formats and content for the unprocessed (RAW) data.  

##### SomaScan submission
**.adat file**: For every SomaScan submission, the .adat file containing the RAW reads, and protein quantification without normalization must be provided. In addition, normalized versions of the .adat file could. 

##### Olink NGS-based submission
**.parquet file**: PRIDE-AP archive supports only NGS-based Olink experiments. The NGS-based submissions must provide the corresponding parquet files which include the sample metadata, protein intensities, and  NPX absolute quantification values. 

#### Additional files

**Additional protein reports**: SomaScan and Olink software and technologies can export additional Excel, and CSV tab-delimited files containing protein quantification values and sample metadata reports. These files could be annotated as OTHERs in the PRIDE-AP database during the submission using the PRIDE Submission tool. 

**File reports**: SomaScan and Olink software and technology release additional files, including QC and quantitative results reports in PDF or Word documents. The PRIDE Submission tool can add these documents to the dataset submission.  

**Experimental Design**: Every Affinity proteomics technology and software releases the experimental design in their file format (e.g. excel file). Those files include the sample and data metadata, and the the experimental design. By January 2024, these files could be provided as additional metadata (file class OTHERS). PRIDE team is working to extend the Proteomics sample metadata format to capture Affinity proteomics datasets.   

## Ownership, privacy and release of datasets to the public

This section is similar to [ProteomeXchange guidelines](https://www.proteomexchange.org/docs/guidelines_px.pdf) for data MS-based proteomics submissions. 

**Data Ownership**: Original submitters and Principal Investigators (PI) own datasets submitted to PRIDE-AP resource. The submitter and PI must be explicitly identified for each dataset.

**Privacy**: Submitted datasets are private by default, with password protection. Reviewers and editors can access data during the journal review process with provided credentials.

**Changes to Datasets**: Changes to unreleased datasets are allowed upon reviewer request. However, after public release, changes must be justified and approved by PRIDE Team.

**Withdrawal**: Unreleased datasets can be withdrawn before publication. Withdrawn entries are retained internally to prevent external access. Publicly released datasets can only be withdrawn in cases of retracted papers.

**Public Release**: Public release of datasets is mandatory upon publication. Exceptions may be considered on a case-by-case basis. Pre-prints are not considered publications, allowing delayed release until formal acceptance.

**Release Responsibility**: PX resources actively release datasets corresponding to known publications without submitters' permission. Authors should be aware that some journals may have similar release policies.

**Timely Release**: PX resources aim for prompt public release, but delays may occur due to production cycles and staff availability.

**Exceptions**: Exceptions to the public release policy may be granted in documented special cases, with a maximum 6-month extension for ongoing studies. Submitters must officially request an extension, considering journal requirements.

You can read more about ProteomeXchange guidelines [here](https://www.proteomexchange.org/docs/guidelines_px.pdf). 

## ProteomeXchange

By January 2024, only MS-based experiments are supported by ProteomeXchange, and  PX accessions are only released for MS-based experiments. Therefore, affinity proteomics datasets stored in PRIDE will not be a part of ProteomeXchange, and the PRIDE database will independently release their accessions. PRIDE team is collaborating with all ProteomeXchange partners to extend support for affinity proteomics submissions in the future. 

## Conclusions
In conclusion, the PRIDE Affinity Proteomics Archive (PRIDE-AP) serves as a pivotal platform dedicated to advancing the field of affinity proteomics. By centralising datasets generated through cutting-edge technologies like Olink and SomaScan, PRIDE-AP aims to facilitate collaboration, accelerate research, and enhance the widespread dissemination of high-quality affinity proteomics data. This document provides comprehensive submission guidelines for datasets, emphasizing the importance of adherence to PRIDE Archive guidelines and policies. 

The unique PAD accession identifier assigned to each dataset ensures proper citation in scientific literature. The document details the specific requirements for dataset structure and file formats, highlighting distinctions between SomaScan and Olink NGS-based submissions. Additional files, experimental design, and metadata considerations are outlined to ensure comprehensive data representation. PRIDE-AP operates independently of ProteomeXchange for affinity proteomics datasets and releases accessions autonomously. The PRIDE team is working with ProteomeXchange partners to enhance support for affinity proteomics submissions and improve accessibility.
