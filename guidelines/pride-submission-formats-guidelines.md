# PRIDE Database Submission Guidelines

Document version: v1.0.0

Authors: 
  - Yasset Perez-Riverol (EMBL-EBI)

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

This document provides comprehensive guidelines for submitting proteomics data to the PRIDE (PRoteomics IDEntifications) database, focusing on the supported file formats and specific requirements for different search engines and analysis tools. Following these guidelines will ensure your data is properly documented, can be effectively reused by the research community, and complies with journal data availability requirements.

## Overview of PRIDE Database

[PRIDE](https://www.ebi.ac.uk/pride/archive) is a centralized repository for mass spectrometry-based proteomics data, including raw data, processed results, and associated metadata. It is part of the ProteomeXchange (PX) Consortium, which provides a coordinated submission process to proteomics repositories. PRIDE focuses on enabling data reuse, reproducibility, and transparency in proteomics research.

## PRIDE Submission Types and File Requirements

PRIDE supports two types of submissions: **PARTIAL** Submissions and **COMPLETE** Submissions. These categories are defined by the ProteomeXchange Consortium, of which PRIDE Archive is an active member. The main difference between the two submission types is that **COMPLETE** Submissions require standard file formats (such as mzIdentML or mzTab) for the results files, while **PARTIAL** Submissions primarily use tool-specific formats.

This document focuses on which file formats and extensions should be provided for **PARTIAL** PRIDE submissions, with **COMPLETE** submissions representing an extension of **PARTIAL** requirements. This document is coordinated between the PRIDE team and software producers and developers of computational proteomics tools to ensure their data is well represented in the archive.

**Note:** The term **PARTIAL** is a convention from ProteomeXchange and is not related to the quality or completeness of the dataset, but rather to the availability of results in a standard file format.

### File Requirements by Submission Type

| File Category | File Type/Format | PARTIAL Submission | COMPLETE Submission | Notes |
|--------------|------------------|-------------------|-------------------|-------|
| **Raw Data Files** | Raw instrument files (.raw, .mzML, .mzXML, .wiff, .baf, .tdf, .d folders) | **Mandatory** | **Mandatory** | Original unprocessed data from mass spectrometer |
| **SEARCH Files** | Search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, etc.) | **Mandatory** | Recommended | Native output from analysis tools (MaxQuant, DIA-NN, FragPipe, etc.). Can be provided as individual files or grouped together in a ZIP or TAR.GZ archive |
| **Peaks Type** | Peak list files (.mgf, .mzML, .mzXML, .dta, .pkl, etc.) | Recommended | Recommended | Processed peak lists extracted from raw data |
| **RESULT Files - Standard Formats** | mzIdentML (.mzid) or mzTab (.mztab) | Recommended | **Mandatory** | Standard format for identification and quantification results |
| **FASTA Database** | Protein sequence database (.fasta, .fa) | Recommended | Recommended | Database used for search (or clear reference to public database) |
| **Spectral Libraries** | Spectral library files (.blib, .sptxt, etc.) | Recommended | Recommended | If used in the analysis workflow |
| **Workflow Files** | Workflow description files (.sky, .skyd, .pdProcessingWF, etc.) | Recommended | Recommended | Tool-specific workflow and method files |
| **Metadata** | Sample to Data Relationship Format (.sdrf.tsv) | **Mandatory** | **Mandatory** | Comprehensive metadata required for all submissions |
| **OTHER** | Supplementary files (.pdf, .doc, .docx, figures, images, etc.) | Recommended | Recommended | Additional documentation, figures, and supplementary materials |

### File Compression Rules

Proper file compression is essential for efficient data storage and transmission when submitting to PRIDE. The following rules apply to all file types:

#### Supported Compression Formats

- **Only ZIP (.zip) and TAR.GZ (.tar.gz) compression formats are supported**
- **RAR (.rar) format is NOT accepted**

#### RAW Files Compression Rules

**Critical: One run per compressed file**

When compressing RAW files (e.g., .raw, .mzML, .mzXML, .wiff files) using ZIP or GZ compression:
- **Only one run should be included per compressed archive**
- **Do NOT compress multiple runs together** into a single ZIP or GZ file

**Why this matters:**
- Each msrun in the SDRF (Sample to Data Relationship Format) must be properly linked to its specific raw file, even when compressed
- Download statistics and links to other omics files are tracked accurately on a per-run basis
- File-level metadata and associations remain intact

**Exception:** This restriction does **NOT** apply to .d folders (Bruker and Agilent), as these directory-based formats are typically associated with a single run per folder and should be compressed as complete folders.

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

#### SEARCH Files Compression Rules

- **All SEARCH files for a given analysis can be grouped together** and compressed into a single ZIP (.zip) or TAR.GZ (.tar.gz) archive
- This is **recommended** for better organization and easier submission
- Group all search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, etc.) from a single analysis into one archive

**Example:**
```bash
zip search_results.zip evidence.txt peptides.txt proteinGroups.txt parameters.txt
```

#### RESULT Files (Standard Formats) Compression

- For large mzIdentML files: Use GZ compression (`gzip large_file.mzid`)
- For multiple mzTab files: Group and ZIP by experiment or analysis batch

#### General Compression Guidelines

1. **Preserve directory structure** when compressing folders
2. **Use logical naming** (e.g., `mascot_exp1.zip`, `maxquant_full_results.tar.gz`)
3. **Size considerations**: Try to keep individual compressed files under 50GB for easier upload
4. **For very large datasets**: Split into multiple compressed archives of reasonable size (< 50GB each), but maintain the one-run-per-archive rule for RAW files

**Note:** These compression rules are explained in detail in the [File Compression Strategies](#file-compression-strategies) section below, but are summarized here for quick reference.

## Supported Raw Data Formats

In proteomics, RAW files are the proprietary data files generated by mass spectrometers, particularly from instruments made by Thermo Fisher Scientific (hence often referred to as Thermo RAW files).

### What RAW files contain:
RAW files store the raw, unprocessed mass spectrometry data, including:

 - MS1 spectra (precursor ions)
 - MS2 spectra (fragmented ions used for peptide identification)
 - Instrument settings (e.g., acquisition method, scan parameters)
 - Metadata (e.g., sample name, run time, calibration details)

### Why RAW files are important:

 - They are the starting point for downstream analysis, such as peptide/protein identification and quantification.
 - Tools like Proteome Discoverer, MaxQuant, and MSConvert (from ProteoWizard) are used to read or convert RAW files into open formats like mzML or mzXML for compatibility with a wider range of software tools.

### Common workflows:

Raw data files contain the unprocessed data directly from the mass spectrometer. PRIDE accepts the following formats by instrument/vendor:

| Format | Extension | Vendor/Source | Description |
|--------|-----------|---------------|-------------|
| Thermo RAW | .raw | Thermo Fisher Scientific | Binary format for Thermo instruments |
| Bruker BAF/TDF | .baf, .tdf, .d folder | Bruker | Format for Bruker instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives (see details below) |
| SCIEX WIFF | .wiff, .wiff2, .scan | SCIEX | Format for SCIEX instruments |
| Agilent D | .d folder | Agilent | Format for Agilent instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives (see details below) |
| Waters RAW | .raw | Waters | Format for Waters instruments |
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

## Search Engine and Analysis Tool Requirements

The following sections detail the specific file formats and requirements for each major proteomics analysis tool and search engine. For each tool, we list the required files, recommended additional files, and important notes to ensure your submission is complete and reusable.

### MaxQuant
*Submission Type: Mostly PARTIAL Submission*

MaxQuant is a software suite for quantitative proteomics, developed to process and analyze large-scale mass spectrometry data.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw) | **Mandatory** | Original mass spectrometry raw data files |
| evidence.txt | **Mandatory** | Evidence table with peptide identifications and quantifications |
| peptides.txt | **Mandatory** | Peptide-level identification and quantification results |
| proteinGroups.txt | **Mandatory** | Protein group identification and quantification results |
| parameters.txt | **Mandatory** | Processing parameters used in the analysis |
| summary.txt | **Mandatory** | Summary statistics of the MaxQuant run |
| mqpar.xml | **Mandatory** | MaxQuant parameter file containing all search settings |
| msms.txt | Recommended | MS/MS scan information and fragment ion matches |
| Modified peptides.txt | Recommended | Modified peptide identifications |
| mzIdentML export (.mzid) | Recommended | Standard format export of identification results (if available) |

**Notes:**
- Include MaxQuant version used
- FASTA database used for the search (or clear reference to public database version)
- Document LFQ settings if applicable
- Special parameters for match-between-runs or other advanced features

### DIA-NN
*Submission Type: PARTIAL Submission*

DIA-NN is an advanced software for data-independent acquisition (DIA) proteomics, using neural networks to improve protein identification and quantification from complex mass spectrometry data.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw or instrument-specific formats) | **Mandatory** | Original mass spectrometry raw data files |
| report.tsv (or report.pr_matrix.tsv) | **Mandatory** | DIA-NN report file with peptide-level results (or precursor matrix) |
| report.pg_matrix.tsv | **Mandatory** | Protein group matrix with quantification results |
| Parameter configuration file | **Mandatory** | DIA-NN parameter configuration file |
| Spectral library (.blib, .sptxt, etc.) | Recommended | Spectral library used for library-based search (if applicable) |
| FASTA database (.fasta, .fa) | Recommended | Protein sequence database used for direct database search (if applicable) |
| Log file | Recommended | DIA-NN analysis log file |

**Notes:**
- Include DIA-NN version
- Document extraction and quantification settings
- Window scheme used for acquisition

### FragPipe/MSFragger
*Submission Type: Mostly PARTIAL Submission*

FragPipe is a versatile platform for mass spectrometry proteomics analysis, offering a user-friendly GUI and command-line options across Windows, Linux, and cloud. It integrates multiple tools and is powered by MSFragger, a fast search engine for both standard and wide-tolerance peptide identification.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw) | **Mandatory** | Original mass spectrometry raw data files |
| pepXML files (.pep.xml) | **Mandatory** | Peptide identification results in pepXML format |
| Protein inference results (.tsv or .txt) | **Mandatory** | Protein-level identification and inference results |
| FragPipe parameter files | **Mandatory** | FragPipe configuration and parameter files |
| mzIdentML export (.mzid) | Recommended | Standard format export of identification results (if available) |
| FASTA database (.fasta, .fa) | Recommended | Protein sequence database used for the search |
| PTM Shepherd output | Recommended | Post-translational modification analysis results (if PTM Shepherd was used) |
| IonQuant output | Recommended | IonQuant quantification results (if IonQuant was used) |

**Notes:**
- Include MSFragger and FragPipe versions
- Document all search parameters, especially for open searches
- Note FDR calculation method

### Skyline
*Submission Type: PARTIAL Submission*

Skyline is a free, open-source Windows application designed for creating and analyzing Selected Reaction Monitoring (SRM)/Multiple Reaction Monitoring (MRM), Parallel Reaction Monitoring (PRM), Data Independent Acquisition (DIA/SWATH), and DDA with MS1 quantitative workflows from mass spectrometry data.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw or vendor formats) | **Mandatory** | Original mass spectrometry raw data files |
| Skyline document (.sky) | **Mandatory** | Skyline document containing method and target definitions |
| Skyline data file (.skyd) | **Mandatory** | Skyline data file with extracted chromatographic data |
| Exported reports (.csv or .tsv) | **Mandatory** | Exported quantification reports in CSV or TSV format |
| Spectral library (.blib) | Recommended | Spectral library used for method development |
| Acquisition method (.method) | Recommended | Instrument acquisition method file (if relevant) |

**Notes:**
- Include Skyline version used
- Document which Skyline plugins were used
- Describe peak integration settings
- Note interference removal techniques if applied

### OpenMS
*Submission Type: Mostly PARTIAL Submission*

OpenMS is an open-source C++ library with Python bindings for managing, analyzing, and visualizing LC/MS data. It facilitates fast development of mass spectrometry software and is freely available under the three-clause BSD license, compatible with Windows, macOS, and Linux.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw) | **Mandatory** | Original mass spectrometry raw data files |
| Analysis results in idXML (.idxml) | **Mandatory** | OpenMS identification results in idXML format |
| Quantification results | **Mandatory** | Quantification results (if quantification was performed) |
| TOPP workflow or KNIME workflow description | **Mandatory** | Workflow description file documenting the analysis pipeline |
| mzIdentML export (.mzid) | Recommended | Standard format export of identification results |
| mzTab export (.mztab) | Recommended | Standard format export of identification and quantification results |
| Parameter files for individual TOPP tools | Recommended | Parameter files for each TOPP tool used in the workflow |

**Notes:**
- Include OpenMS version
- Document which TOPP tools were used
- Describe workflow parameters

### Mascot
*Submission Type: COMPLETE Submission*

Mascot is a widely used software search engine that identifies proteins by matching mass spectrometry data to peptide sequence databases.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw, .mzML) | **Mandatory** | Original mass spectrometry raw data files |
| Mascot DAT files (.dat) | **Mandatory** | Mascot search result files in DAT format |
| mzIdentML export (.mzid) | **Mandatory** | Results exported in mzIdentML standard format (strongly recommended) |
| Parameter file (.xml) | **Mandatory** | Mascot search parameter file |
| MGF files (.mgf) | Recommended | MGF peak list files used for the Mascot search |
| FASTA database (.fasta, .fa) | Recommended | Protein sequence database used (or clear reference to public database version) |
| Custom modifications list | Recommended | List of custom modifications used (if applicable) |

**Notes:**
- Include the version of Mascot Server used
- Specify fixed and variable modifications
- Document all search parameters

### Proteome Discoverer
*Submission Type: PARTIAL Submission*

Thermo Scientific Proteome Discoverer is a flexible software suite for protein research, offering customizable workflows to simplify protein identification, quantification, PTM analysis, isobaric tagging, and label-free quantitation in complex samples.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw) | **Mandatory** | Original mass spectrometry raw data files |
| Proteome Discoverer result file (.pdresult) | **Mandatory** | Proteome Discoverer result file containing all analysis data |
| PSMs export (.txt) | **Mandatory** | Exported PSM (Peptide Spectrum Match) results |
| Peptides export (.txt) | **Mandatory** | Exported peptide-level identification and quantification results |
| Proteins export (.txt) | **Mandatory** | Exported protein-level identification and quantification results |
| mzIdentML export (.mzid) | **Mandatory** | Standard format export of identification results (strongly recommended) |
| Processing workflow (.pdProcessingWF) | Recommended | Proteome Discoverer processing workflow file |
| Consensus workflow (.pdConsensusWF) | Recommended | Proteome Discoverer consensus workflow file |
| FASTA database (.fasta, .fa) | Recommended | Protein sequence database used (or clear reference) |

**Notes:**
- Include Proteome Discoverer version
- Document which nodes and search engines were used in the workflow
- Note scoring thresholds and FDR settings

### MS-GF+
*Submission Type: Mostly PARTIAL Submission*

MS-GF+ is a peptide identification tool that matches MS/MS spectra to protein database peptides. It supports mzML input, outputs mzIdentML, and is compatible with ProteomeXchange submissions.

| File | Status | Description |
|------|--------|-------------|
| Raw data files (.raw) | **Mandatory** | Original mass spectrometry raw data files |
| Result files (.mzid preferred, or .tsv) | **Mandatory** | MS-GF+ search results in mzIdentML format (preferred) or TSV format |
| Parameter file | **Mandatory** | MS-GF+ search parameter file |
| FASTA database (.fasta, .fa) | Recommended | Protein sequence database used (or clear reference) |
| Converted peak lists | Recommended | Peak list files used for the search (if conversion was performed) |

**Notes:**
- Include MS-GF+ version
- Document all search parameters including fragmentation method
- Note any pre-processing steps

### Other Tools

For tools not listed above, generally include:
1. Raw data files in original instrument format
2. Processed peak lists used for searching
3. Search results in original format
4. Converted results to community standard format (mzIdentML, mzTab) when possible
5. Parameter files or detailed documentation of settings
6. FASTA database used or clear reference

## Metadata Requirements

All submissions must include comprehensive metadata:

- **Sample Information**:
  - Species
  - Tissue/cell type
  - Sample preparation protocol
  - Biological/technical replicates structure

- **Instrument Information**:
  - Mass spectrometer type and model
  - Chromatography setup
  - Acquisition method (DDA, DIA, PRM, SRM)

- **Experimental Design**:
  - Description of experimental groups
  - Treatment conditions
  - Labeling strategy (if applicable)
  - Fractionation details (if applicable)

- **Search Parameters**:
  - Database used (version and source)
  - Enzyme specificity
  - Fixed and variable modifications
  - Mass tolerances
  - FDR settings

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
- [ ] Complete metadata
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
| Waters RAW | No compression (maintain as .raw) | These are already in a compressed format |
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

### Search Results and Analysis Tool Outputs

**Note:** All SEARCH files for a given analysis can be grouped together and compressed into a single ZIP (.zip) or TAR.GZ (.tar.gz) archive. This is recommended for better organization and easier submission.

| Tool | Recommended Compression | Strategy |
|------|-------------------------|----------|
| MaxQuant | ZIP or TAR.GZ the entire output folder | Group all txt files and the andromeda folder into a single archive named `maxquant_results.zip` |
| Mascot | ZIP multiple .dat files together | Group by experiment/project |
| DIA-NN | ZIP all TSV/CSV report files | Include all matrices and report files in one archive |
| Skyline | No compression for .sky/.skyd | These are already in a compressed format |
| FragPipe | ZIP all output directories | Group by experiment/search |
| TPP | ZIP all output XML files | Combine pepXML, protXML and other outputs |
| General SEARCH Files | ZIP or TAR.GZ all SEARCH files together | Group all search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, etc.) from a single analysis into one archive |


### mzIdentML and mzTab Files

- For large mzIdentML files: Use GZ compression (`gzip large_file.mzid`)
- For multiple mzTab files: Group and ZIP by experiment or analysis batch

### FASTA and Supplementary Files

- FASTA databases: Always compress with GZ (`gzip database.fasta`)
- Multiple supplementary files: Group logically and ZIP together

### General Compression Recommendations

1. **Group Related Files**: 
   - Group all results from a single analysis tool together
   - Use logical naming (e.g., `mascot_exp1.zip`, `maxquant_full_results.tar.gz`)

2. **Size Considerations**:
   - Try to keep individual compressed files under 50GB for easier upload
   - For very large datasets, split logically by experiment/fraction

3. **Documentation**: 
   - Include a README.txt file in each compressed archive explaining contents
   - Document the compression method used in your submission metadata

4. **Verification**:
   - Always verify the integrity of compressed archives before submission
   - Test extracting files from your archives to ensure they're correctly formed

5. **Directory Structure**:
   - Maintain a consistent directory structure across compressed files
   - Use relative paths within archives

**Note**: These guidelines are regularly updated to reflect new tools and formats. Please check the PRIDE website for the most current version of the submission guidelines.

## Contact and Support

For specific questions about file formats or submission requirements, contact the PRIDE team:
- **Email**: [pride-support@ebi.ac.uk](mailto:pride-support@ebi.ac.uk)
- **Website**: [PRIDE website](https://www.ebi.ac.uk/pride/)
- **Submission Tool**: [PRIDE Submission Tool](https://www.ebi.ac.uk/pride/markdownpage/pridesubmissiontool)
