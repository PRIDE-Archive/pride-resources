# PRIDE Database Submission Guidelines

Document version: v1.0.0

Authors: 
  - Yasset Perez-Riverol (EMBL-EBI)
  - Deepti Jaiswal Kundu (EMBL-EBI)

## Table of Contents
1. [Introduction](#introduction)
2. [Overview of PRIDE Database](#overview-of-pride-database)
3. [PRIDE Submission Types and File Requirements](#pride-submission-types-and-file-requirements)
4. [Supported Raw Data Formats](#supported-raw-data-formats)
5. [Search Engine and Analysis Tool Requirements](#search-engine-and-analysis-tool-requirements)
   - [Mascot](#mascot)
   - [MaxQuant](#maxquant)
   - [DIA-NN](#dia-nn)
   - [Proteome Discoverer](#proteome-discoverer)
   - [MS-GF+](#ms-gf)
   - [Skyline](#skyline)
   - [FragPipe/MSFragger](#fragpipe-msfragger)
   - [OpenMS](#openms)
   - [Other Tools](#other-tools)
6. [Metadata Requirements](#metadata-requirements)
7. [Best Practices for Submission](#best-practices-for-submission)
8. [File Compression Strategies](#file-compression-strategies)
9. [Submission Checklist](#submission-checklist)
10. [Contact and Support](#contact-and-support)

## Introduction

This document provides comprehensive guidelines for submitting proteomics data to the PRIDE (PRoteomics IDEntifications) database, focusing on the supported file formats and specific requirements for different search engines and analysis tools. Following these guidelines will ensure your data is properly documented, can be reused more effectively by the research community, and complies with journal data availability requirements.

## Overview of PRIDE Database

[PRIDE](https://www.ebi.ac.uk/pride) is a centralized repository for mass spectrometry (MS)-based proteomics data, including raw data, processed results, and associated metadata. It is part of the ProteomeXchange (PX) Consortium, which provides a coordinated submission and data dissemination process to proteomics repositories. PRIDE focuses on enabling data reuse, reproducibility, and transparency in proteomics research.

## PRIDE Submission Types and File Requirements

[PRIDE](https://www.ebi.ac.uk/pride) supports two types of submissions: **PARTIAL** Submissions and **COMPLETE** Submissions. These categories are defined by the ProteomeXchange Consortium, of which PRIDE is an active member. The main difference between the two submission types is that **COMPLETE** Submissions require standard file formats (such as mzIdentML or mzTab) for the processed results files, while **PARTIAL** Submissions primarily use tool-specific file formats for both. In both cases, MS raw data is mandatory.

This document focuses on which file formats and extensions should be provided for **PARTIAL** PRIDE submissions, with **COMPLETE** submissions representing an extension of **PARTIAL** submission requirements. This document is coordinated between the PRIDE team and software producers and developers of computational proteomics tools to ensure their data is well represented in PRIDE.

**Note:** The term **PARTIAL** is a convention from ProteomeXchange and is not related to the quality or the completeness of the dataset, but rather to the availability of processed results in a standard file format.

### File Requirements by Submission Type

| File Category | File Type/Format | PARTIAL Submission | COMPLETE Submission | Notes |
|--------------|------------------|-------------------|-------------------|-------|
| **Raw Data Files** | Raw instrument files (.raw, .mzML, .mzXML, .wiff, .baf, .tdf, .d folders) | **Mandatory** | **Mandatory** | Original unprocessed data from mass spectrometer |
| **SEARCH Files** | Search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, etc.) | **Mandatory** | Recommended | Native output from analysis tools (MaxQuant, DIA-NN, FragPipe, etc.). Can be provided as individual files or grouped together in a ZIP or TAR.GZ archive |
| **Peaks Type** | Peak list files (.mgf, .mzML, .mzXML, .dta, .pkl, etc.) | Recommended | Recommended | Processed peak lists extracted from raw data |
| **RESULT Files - Standard Formats** | mzIdentML (.mzid) or mzTab (.mztab) | Not available | **Mandatory** | Standard format for identification and quantification results |
| **FASTA Database** | Protein sequence database (.fasta, .fa) | Recommended | Recommended | Database used for search (or clear reference to public database) |
| **Spectral Libraries** | Spectral library files (.blib, .sptxt, etc.) | Recommended (if applicable) | Recommended (if applicable) | If used in the analysis workflow |
| **Workflow Files** | Workflow description parameters | Recommended | Recommended | Tool-specific workflow and method files |
| **Metadata** | Sample to Data Relationship Format (.sdrf.tsv) | **Mandatory** | **Mandatory** | Comprehensive metadata required for all submissions |
| **OTHER** | Supplementary files (.pdf, .doc, .docx, figures, images, etc.) | Recommended | Recommended | Additional documentation, figures, and supplementary materials |

### File Compression Rules

Proper file compression is essential for efficient data storage and transmission when submitting to PRIDE, _particularly for large files_. The following rules apply to all file types:

#### Supported Compression Formats

- **Only ZIP (.zip), GZ (.gz), and TAR.GZ (.tar.gz) compression formats are supported**
- **RAR (.rar) format is NOT accepted**

#### RAW MS Files Compression Rules

**Critical: One run per compressed file**

When compressing RAW files (e.g., .raw, .mzML, .mzXML, .wiff files) using ZIP or GZ compression:
- **Only one MS run should be included per compressed archive**
- **Do NOT compress multiple runs together** into a single ZIP or GZ file

**Why this matters:**
- Each MS run in the SDRF (Sample to Data Relationship Format) must be properly linked to its specific raw file, even when compressed
- Download statistics and links to other omics files are tracked accurately on a per-file (e.g, MS run) basis
- File-level metadata and associations remain intact

**Exception:** This restriction does **NOT** apply to .d folders (Bruker and Agilent) representing MS Raw data, as these directory-based formats are typically associated with a single run per folder and should be compressed as complete folders.

**Example:**
```bash
# Correct: One run per compressed file
gzip run1.mzML          # Creates run1.mzML.gz
gzip run2.mzML          # Creates run2.mzML.gz

# Incorrect: Multiple runs in one archive
zip all_runs.zip run1.mzML run2.mzML run3.mzML  # DO NOT DO THIS
```

#### Directory-Based RAW Files (.d folders)

- Bruker `.d` folders and Agilent `.d` folders **must be compressed** (as `.zip` or `.tar.gz`) before submission
- Compress the **entire folder** as a single archive, preserving the complete directory structure
- These folders are typically associated with one run, so this is acceptable

**Example:**
```bash
tar -czf experiment1.d.tar.gz experiment1.d/
# or
zip -r experiment1.d.zip experiment1.d/
```

#### SEARCH (Data analysis) Files Compression Rules (for PARTIAL Submissions)

- **All SEARCH files for a given analysis can be grouped** and compressed into a single ZIP (.zip) or TAR.GZ (.tar.gz) archive
- This is **recommended** for better organization and easier submission
- Group all search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, etc.) from a single analysis into one archive

**Example:**
```bash
zip search_results.zip evidence.txt peptides.txt proteinGroups.txt parameters.txt
```

#### RESULT Files (Standard Formats) Compression Results for COMPLETE Submissions

- For large mzIdentML files: Use GZ compression (`gzip large_file.mzid`)
- For multiple mzTab files: Group and ZIP by experiment or analysis batch

#### General Compression Guidelines

1. **Preserve directory structure** when compressing folders
2. **Use logical naming** (e.g., `mascot_exp1.zip`, `maxquant_full_results.tar.gz`)
3. **Size considerations**: Try to keep individual compressed files under 50GB for easier PRIDE upload
4. **For very large datasets**: Split into multiple compressed archives of reasonable size (up to 50GB each), but maintain the one-run-per-archive rule for RAW files

**Note:** These compression rules are explained in detail in the [File Compression Strategies](#file-compression-strategies) section below, but are summarized here for quick reference.

## Supported Raw Data Formats

In proteomics, RAW files are the proprietary data files containing the MS data, as generated by mass spectrometers, particularly from instruments made by Thermo Fisher Scientific (hence often referred to as Thermo RAW files).

### What RAW files contain:
RAW files store the raw, unprocessed mass spectrometry data, including:

 - MS1 spectra (precursor ions)
 - MS2 spectra (fragmented ions used for peptide identification)
- Chromatogram information
 - Instrument settings (e.g., acquisition method, scan parameters)
 - Metadata (e.g., sample name, run time, instrument calibration details)

### Why RAW files are important:

 - They are the starting point for downstream data analysis, such as peptide/protein identification and quantification.
 - Tools like Proteome Discoverer, MaxQuant, and MSConvert (from ProteoWizard) are used to read directly or to convert RAW files into open formats like mzML or mzXML for interoperability with a wider range of software tools.

### Common workflows:

Raw data files contain the unprocessed data directly from the mass spectrometer. PRIDE accepts the following formats by instrument/vendor:

| Format | Extension | Vendor/Source | Description |
|--------|-----------|---------------|-------------|
| Thermo RAW | .raw | Thermo Fisher Scientific | Binary format for Thermo instruments |
| Bruker BAF/TDF | .baf, .tdf, .d folder, .fid | Bruker | Format for Bruker instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives (see details below) |
| SCIEX WIFF | .wiff, .wiff2, .scan | SCIEX | Format for SCIEX instruments |
| Agilent D | .d folder | Agilent | Format for Agilent instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives |
| Waters RAW | .raw | Waters | Format for Waters instruments. **Note:** If RAW files are generated as a directory then you must compress them individually |
| mzML | .mzml | PSI Standard | Open XML-based standard format |

### Directory-Based Raw Data Formats (Bruker and Agilent)

Both Bruker `.d` folders and Agilent `.d` folders must be compressed (as `.zip` or `.tar.gz`) before submission to PRIDE. **Note: RAR format (.rar) is not supported—only ZIP (.zip) and TAR.GZ (.tar.gz) formats are accepted.** The compressed archive must preserve the complete directory structure and include all required files.

#### Bruker .d Folder Contents

When compressing a Bruker `.d` folder, ensure the archive contains the complete folder structure with all of the following files:

#### Agilent .d Folder Contents

When compressing an Agilent `.d` folder, ensure the archive contains the complete folder structure with all of the following files:


**Required files:**
- `AcqData/` directory (contains acquisition data)
  - `.ms` files (mass spectrometry data files)
  - `.msb` files (mass spectrometry binary files)
  - `AcqData.ms` (main acquisition data file)
- `Method/` directory (contains method files)
  - `.meth` files (method files)
  - `Method.m` (main method file)
- `LogFiles/` directory (contains log files, if present)
- `Results/` directory (contains results, if present)
- `*.xml` files (metadata and configuration files)
- `*.info` files (information files)

**Additional files that may be present:**
- `AcqData/mspeak.bin` and `AcqData/mspeak.bin.idx` (peak data and index)
- `AcqData/msprofile.bin` and `AcqData/msprofile.bin.idx` (profile data and index)
- `AcqData/msraw.bin` and `AcqData/msraw.bin.idx` (raw data and index)
- `AcqData/msraw.bin.lock` (lock file for raw data)
- `AcqData/msrt.bin` and `AcqData/msrt.bin.idx` (retention time data and index)
- `AcqData/msrt.bin.lock` (lock file for retention time data)
- `AcqData/msrtdc*.bin` and `AcqData/msrtdc*.bin.idx` (retention time data channels, numbered 1-30, with corresponding index and lock files)
- `*.xml` files (various XML configuration and metadata files)
- `*.info` files (information files)


**Important:** 
- When compressing directory-based raw data, always preserve the complete directory structure. Do not compress individual files separately—compress the entire `.d` folder as a single archive.
- **Only ZIP (.zip) and TAR.GZ (.tar.gz) compression formats are supported. RAR (.rar) format is not accepted.**

## Requieremtns for specific Search Engine and Analysis Tool

The following sections detail the specific file formats and requirements for each major proteomics analysis tool and search engine. For each tool, we list the tool-specific output files, recommended additional files, and important notes to ensure your submission is complete and reusable.

**Note:** MS Raw Data Files and Peak Lists are common requirements for all tools and are covered in the [File Requirements by Submission Type](#file-requirements-by-submission-type) table above. The tables below focus on the tool-specific output files that differ between analysis tools.

### MaxQuant
*Submission Type: Mostly PARTIAL Submission*

MaxQuant is a software suite for quantitative proteomics, which can be used for both Data Dependent-Adquisition (DDA) and Data-Independent Adquisition (DIA).

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| evidence.txt | **Mandatory** | SEARCH | Evidence table with peptide identifications and quantifications |
| peptides.txt | **Mandatory** | SEARCH | Peptide-level identification and quantification results |
| proteinGroups.txt | **Mandatory** | SEARCH | Protein group identification and quantification results |
| parameters.txt | **Mandatory** | OTHER | Processing parameters used in the analysis |
| summary.txt | **Mandatory** | OTHER | Summary statistics of the MaxQuant run |
| mqpar.xml | **Mandatory** | OTHER | MaxQuant parameter file containing all search settings |
| msms.txt | Recommended | SEARCH | MS/MS scan information and fragment ion matches |
| Modified peptides.txt | Recommended | SEARCH | Modified peptide identifications |

### DIA-NN
*Submission Type: PARTIAL Submission*

DIA-NN is an advanced software for DIA proteomics, using neural networks to improve protein identification and quantification.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| report.tsv (or report.pr_matrix.tsv) | **Mandatory** | SEARCH | DIA-NN report file with peptide-level results (or precursor matrix) |
| report.pg_matrix.tsv | **Mandatory** | SEARCH | Protein group matrix with quantification results |
| Parameter configuration file | **Mandatory** | OTHER | DIA-NN parameter configuration file |
| Spectral library (.blib, .sptxt, etc.) | Recommended | SPECTRAL_LIBRARY | Spectral library used for library-based search (if applicable) |
| Log file | Recommended | OTHER | DIA-NN analysis log file |

### FragPipe/MSFragger
*Submission Type: Mostly PARTIAL Submission*

FragPipe is a versatile platform for MS proteomics analysis, offering a user-friendly GUI (Graphical User Interface) and command-line options across Windows, Linux, and cloud. It integrates multiple tools and is powered by MSFragger, a fast search engine for both standard and wide-tolerance peptide identification.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| pepXML files (.pep.xml) | **Mandatory** | SEARCH | Peptide identification results in pepXML format |
| Protein inference results (.tsv or .txt) | **Mandatory** | SEARCH | Protein-level identification and inference results |
| FragPipe parameter files | **Mandatory** | OTHER | FragPipe configuration and parameter files |
| PTM Shepherd output | Recommended | SEARCH | Post-translational modification analysis results (if PTM Shepherd was used) |
| IonQuant output | Recommended | SEARCH | IonQuant quantification results (if IonQuant was used) |

### Skyline
*Submission Type: PARTIAL Submission*

Skyline is a free, open-source Windows application designed for creating and analyzing targeted proteomics workflows such  asSelected Reaction Monitoring (SRM)/Multiple Reaction Monitoring (MRM), Parallel Reaction Monitoring (PRM), Data Independent Acquisition (DIA/SWATH), and DDA with MS1 quantitative workflows from MS data.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Skyline document (.sky) | **Mandatory** | SEARCH | Skyline document containing method and target definitions |
| Skyline data file (.skyd) | **Mandatory** | SEARCH | Skyline data file with extracted chromatographic data |
| Exported reports (.csv or .tsv) | **Mandatory** | SEARCH | Exported quantification reports in CSV or TSV format |
| Spectral library (.blib) | Recommended | SPECTRAL_LIBRARY | Spectral library used for method development |
| Acquisition method (.method) | Recommended | OTHER | Instrument acquisition method file (if relevant) |

### OpenMS
*Submission Type: Mostly PARTIAL Submission*

OpenMS is an open-source C++ library with Python bindings for managing, analyzing, and visualizing LC/MS data. It facilitates fast development of MS software and is freely available under the three-clause BSD license, compatible with Windows, macOS, and Linux.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Analysis results in idXML (.idxml) | **Mandatory** | SEARCH | OpenMS identification results in idXML format |
| Quantification results | **Mandatory** | SEARCH | Quantification results (if quantification was performed) |
| TOPP workflow or KNIME workflow description | **Mandatory** | OTHER | Workflow description file documenting the analysis pipeline |
| mzIdentML export (.mzid) | Recommended | RESULT | Standard format export of identification results |
| mzTab export (.mztab) | Recommended | RESULT | Standard format export of identification and quantification results |
| Parameter files for individual TOPP tools | Recommended | OTHER | Parameter files for each TOPP tool used in the workflow |

### Mascot
*Submission Type: PARTIAL/COMPLETE Submission*

Mascot is a widely used software search engine that identifies proteins by matching MS data to peptide sequence databases.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Mascot DAT files (.dat) | **Mandatory** | SEARCH | Mascot search result files in DAT format |
| mzIdentML export (.mzid) | Recommended | RESULT | Results exported in mzIdentML standard format (strongly recommended) |
| Parameter file (.xml) | **Mandatory** | OTHER | Mascot search parameter file |
| MGF files (.mgf) | Recommended | PEAK | MGF peak list files used for the Mascot search |
| Custom modifications list | Recommended | OTHER | List of custom modifications used (if applicable) |

### Proteome Discoverer
*Submission Type: PARTIAL Submission*

Thermo Scientific Proteome Discoverer is a flexible software suite for proteomics research, offering customizable workflows to simplify protein identification, quantification, PTM analysis, isobaric tagging, and label-free quantitation in complex samples.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Proteome Discoverer result file (.pdresult) | **Mandatory** | SEARCH | Proteome Discoverer result file containing all analysis data |
| PSMs export (.txt) | **Mandatory** | SEARCH | Exported PSM (Peptide Spectrum Match) results |
| Peptides export (.txt) | **Mandatory** | SEARCH | Exported peptide-level identification and quantification results |
| Proteins export (.txt) | **Mandatory** | SEARCH | Exported protein-level identification and quantification results |
| mzIdentML export (.mzid) | Recommended | RESULT | Standard format export of identification results (strongly recommended) |

### MS-GF+
*Submission Type: Mostly PARTIAL/COMPLETE Submission*

MS-GF+ is a peptide identification tool that matches MS/MS spectra to protein sequence databases. It supports mzML input, outputs mzIdentML, and is compatible with ProteomeXchange submissions.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Result files (.mzid) | **Mandatory** | RESULT | MS-GF+ search results in mzIdentML format (preferred) |
| Result files (.tsv) | **Mandatory** | SEARCH | MS-GF+ search results in TSV format (alternative to mzIdentML) |
| Parameter file | Recommended | OTHER | MS-GF+ search parameter file |
| Converted peak lists | Recommended | OTHER | Peak list files used for the search (if conversion was performed) |

### Other Tools

For tools not listed above, generally include:
1. Raw data files in original instrument format
2. Processed peak lists used for searching
3. Search results in original format
4. Converted results to community standard format (mzIdentML, mzTab) when possible
5. Parameter files or detailed documentation of settings

## Sample Metadata Requirements

PRIDE supports and encorage the submission of metadata using the [SDRF-proteomics format](https://github.com/bigbio/proteomics-sample-metadata). This tab-delimited format includes not only the sample metadata but also other information about the data acquisition including for example: instrument settings; enzyme; acquisition mode. 

## Best Practices for Submission

1. **File Organization**: Group files logically by experiment and analysis method.
2. **Standard Formats**: When possible, provide data in both native formats and standard formats (mzIdentML, mzTab).
3. **Complete Documentation**: Ensure all parameters used in data processing are documented either in parameter files or metadata.
4. **Quality Control**: Include QC metrics when available.
5. **Consistent Naming**: Use consistent, descriptive file naming conventions.
6. **Version Information**: Document software versions for all tools used.
7. **File Compression**: Use appropriate compression strategies to reduce storage requirements (see dedicated section below).

## Submission Checklist

- [ ] Raw data files in original format
- [ ] Processed data files (mzIdentML/MGF/etc.)
- [ ] Identification results files
- [ ] Quantification results files (if applicable)
- [ ] Parameter files for all search engines/tools used
- [ ] FASTA database files or precise references
- [ ] Complete metadata (SDRF)
- [ ] QC metrics (if available)
- [ ] README file explaining file organization (recommended)


## File Compression Strategies

Proper file compression is essential for efficient data storage and transmission when submitting to PRIDE. **Important: PRIDE only accepts ZIP (.zip) and TAR.GZ (.tar.gz) compression formats. RAR (.rar) format is not supported.** Below are recommended compression strategies for different file types:

### Raw Data Files

| File Type | Recommended Compression | Notes |
|-----------|-------------------------|-------|
| Thermo RAW | No compression (maintain as .raw) | These are already in a compressed binary format |
| Agilent .d folders | ZIP or TAR.GZ the entire folder | `tar -czf experiment1.d.tar.gz experiment1.d/` |
| Bruker .d folders | ZIP or TAR.GZ the entire folder | Include all subfolders and files |
| Waters RAW | ZIP or TAR.GZ if directory-based; otherwise uncompressed | Compress directory-based .raw folders; single .raw files may remain uncompressed |
| SCIEX WIFF | No compression (maintain as .wiff/.wiff2) | These are already efficiently stored |
| mzML/mzXML | GZ compression (one file per archive) | `gzip large_file.mzML` to create `large_file.mzML.gz`. **Important:** Only compress one run per file—do not compress multiple runs together |

**Important Notes for Raw Data:**
- When compressing directory-based raw data (.d folders), ensure you preserve the directory structure
- PRIDE accepts both uncompressed and compressed raw files
- **Only ZIP (.zip) and TAR.GZ (.tar.gz) compression formats are supported. RAR (.rar) format is not accepted.**
- **Critical: One run per compressed file** - When compressing RAW files (e.g., .raw, .mzML, .mzXML, .wiff files) using ZIP or GZ compression, **only one run should be included per compressed archive**. Do not compress multiple runs together into a single ZIP or GZ file. This requirement ensures that:
  - Each msrun in the SDRF (Sample to Data Relationship Format) can be properly linked to its specific raw file, even when compressed
  - Download statistics and links to other omics files can be tracked accurately on a per-run basis
  - File-level metadata and associations remain intact
- **Note:** This restriction does not apply to .d folders (Bruker and Agilent), as these directory-based formats are typically associated with a single run per folder
- For very large datasets, consider splitting into multiple compressed archives of reasonable size (< 50GB each), but maintain the one-run-per-archive rule

## Contact and Support

**Note**: These guidelines are regularly updated to reflect new tools and formats. Please check the PRIDE website for the most current version of the submission guidelines.

For specific questions about file formats or submission requirements, contact the PRIDE team:
- **Email**: [pride-support@ebi.ac.uk](mailto:pride-support@ebi.ac.uk)
- **Website**: [PRIDE website](https://www.ebi.ac.uk/pride/)
- **Submission Tool**: [PRIDE Submission Tool](https://www.ebi.ac.uk/pride/markdownpage/pridesubmissiontool)
