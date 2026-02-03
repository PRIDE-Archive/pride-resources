# PRIDE Database Submission Guidelines

Document version: v2.0.0

Authors:
- Yasset Perez-Riverol (EMBL-EBI)
- Deepti Jaiswal Kundu (EMBL-EBI)
- Fengchao Yu (University of Michigan)

## Table of Contents
1. [Introduction](#introduction)
2. [Overview of PRIDE Database](#overview-of-pride-database)
3. [Terminology and Historical Context](#terminology-and-historical-context)
4. [PRIDE Submission File Requirements](#pride-submission-file-requirements)
5. [Supported Raw Data Formats](#supported-raw-data-formats)
6. [Search Engine and Analysis Tool Requirements](#search-engine-and-analysis-tool-requirements)
   - [MaxQuant](#maxquant)
   - [DIA-NN](#dia-nn)
   - [Spectronaut](#spectronaut)
   - [FragPipe/MSFragger](#fragpipe-msfragger)
   - [Skyline](#skyline)
   - [OpenMS](#openms)
   - [Mascot](#mascot)
   - [Proteome Discoverer](#proteome-discoverer)
   - [MS-GF+](#ms-gf)
   - [Other Tools](#other-tools)
7. [Metadata Requirements](#metadata-requirements)
8. [Best Practices for Submission](#best-practices-for-submission)
9. [File Compression Strategies](#file-compression-strategies)
10. [Submission Checklist](#submission-checklist)
11. [Contact and Support](#contact-and-support)

## Introduction

This document provides comprehensive guidelines for submitting proteomics data to the PRIDE (PRoteomics IDEntifications) database, focusing on the supported file formats and specific requirements for different search engines and analysis tools. Following these guidelines will ensure your data is properly documented, can be reused more effectively by the research community, and complies with journal data availability requirements.

### What's New in This Version

In this version of the guidelines, we have updated the terminology to better reflect the nature of the files being submitted:

- **ANALYSIS Files** (previously called "SEARCH files"): These are the native output files from search engines and analysis tools (e.g., evidence.txt, peptides.txt, report.tsv). We renamed this category because these files represent the actual results of the computational analysis—the primary outputs that researchers work with and that contain the peptide/protein identifications and quantifications.

- **STANDARD File Formats** (previously called "RESULT files"): These are community-standard formats like mzIdentML (.mzid) and mzTab (.mztab) that provide interoperable representations of identification and quantification data. We renamed this category to emphasize that these formats are standardized representations rather than the primary analysis outputs.

This terminology change reflects our focus on the actual outputs from search engines and analysis pipelines. The ANALYSIS files are the direct outputs of computational workflows—they contain the complete analysis data in the format native to each tool. STANDARD file formats, while valuable for interoperability, are derived representations of these results.

## Overview of PRIDE Database

[PRIDE](https://www.ebi.ac.uk/pride) is a centralized repository for mass spectrometry (MS)-based proteomics data, including raw data, processed results, and associated metadata. It is part of the ProteomeXchange (PX) Consortium, which provides a coordinated submission and data dissemination process to proteomics repositories. PRIDE focuses on enabling data reuse, reproducibility, and transparency in proteomics research.

## Terminology and Historical Context

### ProteomeXchange Background: PARTIAL vs COMPLETE Submissions

The [ProteomeXchange Consortium](http://www.proteomexchange.org/), of which PRIDE is a founding member, has historically classified submissions into two categories:

- **PARTIAL Submissions**: Datasets containing raw data and tool-specific result files (native search engine outputs), but without standardized format files like mzIdentML or mzTab.
- **COMPLETE Submissions**: Datasets that included, in addition to raw data and tool-specific files, standardized format files (mzIdentML or mzTab) that enable automated data validation and integration.

The term "PARTIAL" was a ProteomeXchange convention that, while technically accurate, often caused confusion among submitters. It was **not** related to the quality or completeness of the scientific dataset—a "PARTIAL" submission could contain a perfectly complete and high-quality proteomics experiment. The terminology simply indicated whether standardized format files were included.

### PRIDE's Approach: Focus on Analysis Outputs

These guidelines represent **PRIDE's submission framework**, designed to simplify the submission experience while maintaining full compatibility with ProteomeXchange standards.

**What this means for submitters:**

Within the PRIDE submission process, we adopt a unified approach where **all datasets are simply "submissions"**. We no longer emphasize the PARTIAL/COMPLETE distinction during submission. Instead, we focus on what matters most: your raw data, the native outputs from your analysis tools (ANALYSIS files), and comprehensive metadata.

- **All submissions** require raw data and ANALYSIS files (native tool outputs from software like MaxQuant, DIA-NN, FragPipe, Spectronaut, etc.).
- **Submissions with STANDARD file formats** (mzIdentML, mzTab) benefit from enhanced interoperability and automated validation.
- **Submissions without STANDARD file formats** are equally valid and scientifically complete. Modern analysis tools produce rich, well-structured output files that provide comprehensive information for data reuse.

**ProteomeXchange Compliance:**

PRIDE remains fully compliant with ProteomeXchange guidelines. When your submission includes validated STANDARD file formats (mzIdentML or mzTab), PRIDE will automatically register it as a "COMPLETE" submission with ProteomeXchange. Submissions without these formats will be registered as "PARTIAL" in the ProteomeXchange ecosystem. However, this technical classification happens behind the scenes and does not affect the value or visibility of your dataset.

**Why we emphasize ANALYSIS files:**

We believe the native outputs from search engines and analysis pipelines deserve greater recognition. These files—such as MaxQuant's evidence.txt, DIA-NN's report.parquet, or FragPipe's psm.tsv files—contain the complete results of computational analyses in formats optimized by tool developers. By prioritizing these ANALYSIS files, we ensure that the rich information produced by modern proteomics software is fully captured and preserved for the research community.

## PRIDE Submission File Requirements

This section describes the file types required for PRIDE submissions. The guidelines are coordinated between the PRIDE team and software producers/developers of computational proteomics tools to ensure data is well represented in PRIDE.

### Key File Format Definitions

Before reviewing the file requirements, here are definitions of common file formats you will encounter:

- **mzIdentML (.mzid)**: A standardized XML format developed by the Proteomics Standards Initiative (PSI) for reporting peptide and protein identifications from mass spectrometry data. It captures search engine results, including peptide-spectrum matches, protein inference, and associated scores.

- **mzTab (.mztab)**: A tab-delimited text format developed by the PSI for reporting identification and quantification results. It provides a simpler, more human-readable alternative to mzIdentML and is particularly useful for quantitative proteomics data.

- **pepXML (.pep.xml)**: An XML format originally developed for the Trans-Proteomic Pipeline (TPP) to store peptide identification results. Widely used by tools like FragPipe/MSFragger and the TPP suite.

- **idXML (.idxml)**: An XML format native to OpenMS for storing peptide and protein identification results within OpenMS workflows.

- **MGF (Mascot Generic Format, .mgf)**: A simple text-based format for storing processed peak lists (MS/MS spectra). Commonly used as input for database search engines.

- **SDRF (.sdrf.tsv)**: Sample and Data Relationship Format—a tab-delimited file that describes the relationship between samples, experimental conditions, and data files. Essential for reproducibility and data reuse.

- **Parquet (.parquet)**: A columnar storage format optimized for efficient data processing. Used by DIA-NN for storing large result files with better compression and faster read times than TSV.

### File Requirements

| File Category | File Type/Format | Status | Notes |
|--------------|------------------|--------|-------|
| **Raw Data Files** | Raw instrument files (.raw, .mzML, .mzXML, .wiff, .baf, .tdf, .d folders) | **Mandatory** | Original unprocessed data from the mass spectrometer |
| **ANALYSIS Files** | Search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, .parquet, etc.) | **Mandatory** | Native output from analysis tools (MaxQuant, DIA-NN, FragPipe, Spectronaut, etc.). Can be provided as individual files or grouped in a ZIP or TAR.GZ archive |
| **Peak List Files** | Peak list files (.mgf, .mzML, .mzXML, .dta, .pkl, etc.) | Recommended | Processed peak lists extracted from raw data |
| **STANDARD File Formats** | mzIdentML (.mzid) or mzTab (.mztab) | Recommended | Community-standard formats for identification and quantification results. Including these improves interoperability and enables automated validation |
| **FASTA Database** | Protein sequence database (.fasta, .fa) | **Mandatory** | Database used for search (or clear reference to a public database version) |
| **Spectral Libraries** | Spectral library files (.blib, .sptxt, .tsv, etc.) | Recommended (if applicable) | Required if used in the analysis workflow (e.g., library-based DIA searches) |
| **Workflow Files** | Workflow description and parameters | Recommended | Tool-specific workflow and method files documenting the analysis pipeline |
| **Metadata** | Sample to Data Relationship Format (.sdrf.tsv) | **Recommended** | Comprehensive metadata describing samples, experimental design, and data acquisition |
| **OTHER** | Supplementary files (.pdf, .doc, .docx, figures, images, etc.) | Optional | Additional documentation, figures, and supplementary materials |

**Note:** If an mzTab file fails validation, you can upload it as an ANALYSIS file instead.

Proper file compression is essential for efficient data storage and transmission when submitting to PRIDE, _particularly for .d folders and large files_. The following rules apply to all file types:

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


### Supported Compression Formats

- **Only ZIP (.zip), GZ (.gz), and TAR.GZ (.tar.gz) compression formats are supported**
- **RAR (.rar) format is NOT accepted**

### RAW Files Compression Rules

**Critical: One run per compressed file (for .d folders, one folder per compressed file)**

When compressing RAW files (e.g., .raw, .mzML, .mzXML, .wiff files) using ZIP or GZ compression:
- **Only one MS run should be included per compressed archive**
- **Do NOT compress multiple runs together** into a single ZIP or GZ file

**Why this matters:**
- Each MS run in the SDRF (Sample to Data Relationship Format) must be properly linked to its specific raw file, even when compressed
- Download statistics and links to other omics files are tracked accurately on a per-file (e.g., per MS run) basis
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

#### Directory-Based RAW Files (e.g. .d folders)

- Bruker `.d` folders and Agilent `.d` folders **must be compressed** (as `.zip` or `.tar.gz`) before submission
- Compress the **entire folder** as a single archive, preserving the complete directory structure
- These folders are typically associated with one run, so this is acceptable

**Example:**
```bash
tar -czf experiment1.d.tar.gz experiment1.d/
# or
zip -r experiment1.d.zip experiment1.d/
```

#### ANALYSIS Files Compression Rules

- **All ANALYSIS files for a given analysis can be grouped** and compressed into a single ZIP (.zip) or TAR.GZ (.tar.gz) archive
- This is **recommended** for better organization and easier submission
- Group all search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, .parquet, etc.) from a single analysis into one archive

**Example:**
```bash
zip result_files.zip evidence.txt peptides.txt proteinGroups.txt parameters.txt
```

#### STANDARD File Formats Compression

- For large mzIdentML files: Use GZ compression (`gzip large_file.mzid`)
- For multiple mzTab files: Group and ZIP by experiment or analysis batch

### FASTA and Supplementary Files

1. **Preserve directory structure** when compressing folders
2. **Use logical naming** (e.g., `mascot_exp1.zip`, `maxquant_full_results.tar.gz`)
3. **Size considerations**: Try to keep individual compressed files under 50GB for easier PRIDE upload
4. **For very large datasets**: Split into multiple compressed archives of reasonable size (up to 50GB each), but maintain the one-run-per-archive rule for RAW files


## Supported Raw Data Formats

In proteomics, RAW files are the proprietary data files containing the MS data, as generated by mass spectrometers, particularly from instruments made by Thermo Fisher Scientific (hence often referred to as Thermo RAW files).

### What RAW Files Contain

RAW files store the raw, unprocessed mass spectrometry data, including:

- MS1 spectra (precursor ions)
- MS2 spectra (fragmented ions used for peptide identification)
- Chromatogram information
- Instrument settings (e.g., acquisition method, scan parameters)
- Metadata (e.g., sample name, run time, instrument calibration details)

### Why RAW Files Are Important

- They are the starting point for downstream data analysis, such as peptide/protein identification and quantification.
- Tools like ThermoRawFileParser and MSConvert (from ProteoWizard) can read these files directly or convert them into open formats like mzML or mzXML for interoperability with a wider range of software tools.

Raw data files contain the unprocessed data directly from the mass spectrometer. PRIDE accepts the following formats by instrument/vendor:

| Format | Extension | Vendor/Source | Description |
|--------|-----------|---------------|-------------|
| Thermo RAW | .raw | Thermo Fisher Scientific | Binary format for Thermo instruments |
| Bruker BAF/TDF | .baf, .tdf, .d folder, .fid | Bruker | Format for Bruker instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives (see details below) |
| SCIEX WIFF | .wiff, .wiff2, .scan | SCIEX | Format for SCIEX instruments |
| Agilent D | .d folder | Agilent | Format for Agilent instruments. **Note:** .d folders must be provided as .zip or .tar.gz archives |
| Waters RAW | .raw | Waters | Format for Waters instruments. **Note:** If RAW files are generated as a directory then you must compress them individually |
| mzML | .mzml | PSI Standard | Open XML-based standard format |

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

#### Directory-Based Raw Data Formats (Bruker and Agilent)

When compressing `.d` folders, **include the entire folder with all its contents**. Do not selectively include or exclude files—the complete directory structure is required for the data to be readable.

**Bruker .d folders** (timsTOF and other Bruker instruments):
- Compress the entire `.d` folder as-is
- Key files include `analysis.tdf` (metadata database) and `analysis.tdf_bin` (spectral data) for timsTOF instruments
- Older Bruker formats may contain `.baf` or `.fid` files instead

**Agilent .d folders**:
- Compress the entire `.d` folder as-is
- These typically contain an `AcqData/` directory with acquisition data and a `Method/` directory with method files

**Important:** Do not modify, reorganize, or remove any files from the `.d` folder before compression. The internal structure is vendor-specific and all files may be required for proper data access.

#### Raw Data Validation

PRIDE performs automated validation on submitted raw data files to ensure they are readable and properly formatted. During the submission process:

- **File integrity checks**: Compressed archives are verified to ensure they are not corrupted and can be extracted successfully.
- **Format validation**: Raw files are checked to confirm they contain valid mass spectrometry data and can be read by standard tools.
- **Structure verification**: For directory-based formats (.d folders), the pipeline verifies that essential files are present (e.g., `analysis.tdf` for Bruker timsTOF, `AcqData/` for Agilent).

If validation fails, you will be notified with specific error messages to help you identify and resolve the issue before resubmitting. Common issues include incomplete .d folder compression, corrupted files, or unsupported format versions.

### ANALYSIS Files and Tool Outputs

**Note:** All ANALYSIS files for a given analysis can be grouped together and compressed into a single ZIP (.zip) or TAR.GZ (.tar.gz) archive. This is recommended for better organization and easier submission.

| Tool | Recommended Compression | Strategy |
|------|-------------------------|----------|
| MaxQuant | ZIP or TAR.GZ the entire output folder | Group all txt files and the andromeda folder into a single archive named `maxquant_results.zip` |
| Mascot | ZIP multiple .dat files together | Group by experiment/project |
| DIA-NN | ZIP all output files | Include report.parquet/report.tsv and log files in one archive |
| Spectronaut | ZIP all output files | Include all report files and settings in one archive |
| Skyline | No compression for .sky/.skyd | These are already in a compressed format |
| FragPipe | ZIP output .tsv and .csv files | Group by experiment/search |
| TPP | ZIP all output XML files | Combine pepXML, protXML, and other outputs |
| General ANALYSIS Files | ZIP or TAR.GZ all ANALYSIS files together | Group all search engine output files (.txt, .tsv, .csv, .pep.xml, .idxml, .dat, .pdresult, .parquet, etc.) from a single analysis into one archive |


## Requirements for Specific Search Engines and Analysis Tools

The following sections detail the specific file formats and requirements for each major proteomics analysis tool and search engine. For each tool, we list the tool-specific output files, recommended additional files, and important notes to ensure your submission is complete and reusable.

**Note:** MS Raw Data Files and Peak Lists are common requirements for all tools and are covered in the [File Requirements](#file-requirements) table above. The tables below focus on the tool-specific ANALYSIS files that differ between analysis tools.

### MaxQuant

MaxQuant is a software suite for quantitative proteomics that supports both Data-Dependent Acquisition (DDA) and Data-Independent Acquisition (DIA) workflows.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| sdrf.tsv | **Mandatory** | METADATA | SDRF metadata file describing samples and experimental design |
| mzTab.mzTab | **Mandatory** | ANALYSIS | mzTab output containing identification and quantification data |
| evidence.txt | **Mandatory** | ANALYSIS | Evidence table with peptide identifications and quantifications at the PSM level |
| peptides.txt | **Mandatory** | ANALYSIS | Peptide-level identification and quantification results |
| proteinGroups.txt | **Mandatory** | ANALYSIS | Protein group identification and quantification results |
| parameters.txt | **Mandatory** | OTHER | Processing parameters used in the analysis |
| summary.txt | **Mandatory** | OTHER | Summary statistics of the MaxQuant run |
| mqpar.xml | **Mandatory** | OTHER | MaxQuant parameter file containing all search settings |
| allPeptides.txt | Recommended | ANALYSIS | All peptide-like features detected in the data |
| matchedFeatures.txt | Recommended | ANALYSIS | Matched peptide-like features between runs |
| msScans.txt | Recommended | ANALYSIS | MS scan information |
| msmsScans.txt | Recommended | ANALYSIS | MS/MS scan information and fragment ion matches for identifications |
| Oxidation (M)Sites.txt | Recommended | ANALYSIS | Oxidation modification site identifications |
| modificationSpecificPeptides.txt | Recommended | ANALYSIS | Modified peptide identifications |
| msms.txt | Optional | ANALYSIS | All MS/MS scans processed |
| ms3Scans.txt | Optional | ANALYSIS | MS3 scan information and fragment ion matches |
| fragment.txt | Optional | ANALYSIS | Fragment information |
| fragmentQuant.txt | Optional | ANALYSIS | Fragment quantification data |
| mzRange.txt | Optional | ANALYSIS | Isotope patterns in the data |
| dependentPeptides.txt | Optional | ANALYSIS | Dependent peptide analysis results |


### DIA-NN

DIA-NN is an advanced software for Data-Independent Acquisition (DIA) proteomics that uses neural networks to improve protein identification and quantification accuracy.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| report.parquet (or report.tsv) | **Mandatory** | ANALYSIS | Main DIA-NN report file containing all identifications and quantifications |
| Spectral library (.blib, .sptxt, .tsv, etc.) | **Mandatory** (if used) | SPECTRAL_LIBRARY | Spectral library used for library-based search. Required if library search was performed |
| Log file (.log.txt) | **Mandatory** | OTHER | DIA-NN analysis log file documenting the processing steps |
| Parameter configuration file | Recommended | OTHER | DIA-NN configuration file with search parameters |
| pr_matrix.tsv | Recommended | ANALYSIS | Precursor-level quantification matrix |
| pg_matrix.tsv | Recommended | ANALYSIS | Protein group-level quantification matrix |


### Spectronaut

Spectronaut is a commercial software platform for Data-Independent Acquisition (DIA) proteomics analysis developed by Biognosys. It provides comprehensive DIA data processing, including library-based and directDIA (library-free) analysis workflows, with advanced algorithms for peptide identification and protein quantification.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Main report (.tsv, .csv, or .xls) | **Mandatory** | ANALYSIS | Primary Spectronaut report containing peptide/protein identifications and quantifications |
| Spectral library (.kit or .tsv) | **Mandatory** (if used) | SPECTRAL_LIBRARY | Spectral library used for analysis. Required if library-based search was performed |
| Analysis settings/parameters | **Mandatory** | OTHER | Spectronaut analysis settings file or exported parameters documenting the search configuration |
| Protein group report | Recommended | ANALYSIS | Protein group-level quantification report |
| Peptide report | Recommended | ANALYSIS | Peptide-level quantification report |
| Fragment report | Optional | ANALYSIS | Fragment-level quantification data |
| Candidates report | Optional | ANALYSIS | All candidate identifications before filtering |


### FragPipe/MSFragger

FragPipe is a versatile platform for MS proteomics analysis, offering both a graphical user interface (GUI) and command-line options across Windows, Linux, and cloud environments. It integrates multiple tools including DIA-Umpire, diaTracer, MSFragger, MSBooster, Percolator, PeptideProphet, PTMProphet, ProteinProphet, Philosopher, PTM-Shepherd, IonQuant, TMT-Integrator, EasyPQP, and DIA-NN.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Peptide identification results (psm.tsv) | **Mandatory** | ANALYSIS | Peptide identification results in tsv format |
| Protein inference results (protein.tsv) | **Mandatory** | ANALYSIS | Protein-level identification and inference results |
| FragPipe parameter files (fragpipe.workflow) | **Mandatory** | OTHER | FragPipe configuration and parameter files |
| FragPipe manifest files (fragpipe-files.fp-manifest) | **Mandatory** | OTHER | FragPipe manifest files |
| combined_ion.tsv | Recommended | ANALYSIS | Combined ion-level results across experiments |
| combined_modified_peptide.tsv | Recommended | ANALYSIS | Combined peptidoform-level results across experiments |
| combined_peptide.tsv | Recommended | ANALYSIS | Combined peptide-level results across experiments |
| combined_protein.tsv | Recommended | ANALYSIS | Combined protein-level results across experiments |
| combined_ion_label_quant.tsv | Recommended | ANALYSIS | Combined ion-level results from isotopic labeling quantification |
| combined_modified_peptide_label_quant.tsv | Recommended | ANALYSIS | Combined peptidoform-level results from isotopic labeling quantification |
| combined_protein_label_quant.tsv | Recommended | ANALYSIS | Combined protein-level results from isotopic labeling quantification |
| the whole tmt-report folder | Recommended | ANALYSIS | Combined results from isobaric labeling quantification |

### Skyline

Skyline is a free, open-source Windows application designed for creating and analyzing targeted proteomics workflows such as Selected Reaction Monitoring (SRM)/Multiple Reaction Monitoring (MRM), Parallel Reaction Monitoring (PRM), Data-Independent Acquisition (DIA/SWATH), and DDA with MS1 quantitative workflows.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Skyline document (.sky) | **Mandatory** | ANALYSIS | Skyline document containing method, target definitions, and analysis results |
| Skyline data file (.skyd) | **Mandatory** | ANALYSIS | Skyline data file with extracted chromatographic data |
| Exported reports (.csv or .tsv) | **Mandatory** | ANALYSIS | Exported quantification reports in CSV or TSV format |
| Spectral library (.blib) | Recommended | SPECTRAL_LIBRARY | Spectral library used for method development |
| Acquisition method (.method) | Recommended | OTHER | Instrument acquisition method file (if relevant) |

### OpenMS

OpenMS is an open-source C++ library with Python bindings for managing, analyzing, and visualizing LC/MS data. It facilitates rapid development of MS software and is freely available under the three-clause BSD license, compatible with Windows, macOS, and Linux.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Analysis results in idXML (.idxml) | **Mandatory** | ANALYSIS | OpenMS identification results in idXML format |
| Quantification results (.consensusXML, .featureXML) | **Mandatory** | ANALYSIS | Quantification results (if quantification was performed) |
| TOPP workflow or KNIME workflow description | **Mandatory** | OTHER | Workflow description file documenting the analysis pipeline |
| mzIdentML export (.mzid) | Recommended | STANDARD | Standard format export of identification results |
| mzTab export (.mztab) | Recommended | STANDARD | Standard format export of identification and quantification results |
| Parameter files for individual TOPP tools | Recommended | OTHER | Parameter files for each TOPP tool used in the workflow |

### Mascot

Mascot is a widely used search engine that identifies proteins by matching MS data to peptide sequence databases.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Mascot DAT files (.dat) | **Mandatory** | ANALYSIS | Mascot search result files in DAT format |
| mzIdentML export (.mzid) | Recommended | STANDARD | Results exported in mzIdentML standard format (strongly recommended) |
| Parameter file (.xml) | **Mandatory** | OTHER | Mascot search parameter file |
| MGF files (.mgf) | Recommended | PEAK | MGF peak list files used for the Mascot search |
| Custom modifications list | Recommended | OTHER | List of custom modifications used (if applicable) |

### Proteome Discoverer

Thermo Scientific Proteome Discoverer is a flexible software suite for proteomics research, offering customizable workflows for protein identification, quantification, PTM analysis, isobaric tagging, and label-free quantitation in complex samples.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Proteome Discoverer result file (.pdresult) | **Mandatory** | ANALYSIS | Proteome Discoverer result file containing all analysis data |
| PSMs export (.txt) | **Mandatory** | ANALYSIS | Exported PSM (Peptide Spectrum Match) results |
| Peptides export (.txt) | **Mandatory** | ANALYSIS | Exported peptide-level identification and quantification results |
| Proteins export (.txt) | **Mandatory** | ANALYSIS | Exported protein-level identification and quantification results |
| mzIdentML export (.mzid) | Recommended | STANDARD | Standard format export of identification results (strongly recommended) |

### MS-GF+

MS-GF+ is a peptide identification tool that matches MS/MS spectra to protein sequence databases. It supports mzML input and outputs mzIdentML format natively, making it well-suited for standardized submissions.

| File | Status | File Category | Description |
|------|--------|---------------|-------------|
| Result files (.mzid) | **Mandatory** | STANDARD | MS-GF+ search results in mzIdentML format (preferred and native output) |
| Result files (.tsv) | Optional | ANALYSIS | MS-GF+ search results in TSV format (alternative to mzIdentML) |
| Parameter file | Recommended | OTHER | MS-GF+ search parameter file |
| Converted peak lists | Recommended | PEAK | Peak list files used for the search (if conversion was performed) |

### Other Tools

For tools not listed above, generally include:
1. Raw data files in original instrument format
2. Processed peak lists used for searching (PEAK files)
3. Search results in original/native format (ANALYSIS files)
4. Results converted to community standard formats (mzIdentML, mzTab) when possible (STANDARD files)
5. Parameter files or detailed documentation of settings (OTHER files)

## Metadata Requirements

PRIDE supports and encourages the submission of metadata using the [SDRF-proteomics format](https://github.com/bigbio/proteomics-sample-metadata). This tab-delimited format describes the relationship between samples and data files, and includes information about:

- Sample characteristics (organism, tissue, cell type, disease state, etc.)
- Experimental design and conditions
- Data acquisition parameters (instrument, fragmentation method, etc.)
- Search parameters (enzyme, modifications, etc.)

Including comprehensive metadata greatly enhances the reusability of your data by the research community. 

## Best Practices for Submission

1. **File Organization**: Group files logically by experiment and analysis method.
2. **Standard Formats**: When possible, provide data in both native formats and standard formats (mzIdentML, mzTab).
3. **Complete Documentation**: Ensure all parameters used in data processing are documented either in parameter files or metadata.
4. **Quality Control**: Include QC metrics when available.
5. **Consistent Naming**: Use consistent, descriptive file naming conventions.
6. **Version Information**: Document software versions for all tools used.
7. **File Compression**: Use appropriate compression strategies to reduce storage requirements (see dedicated section below).

## Submission Checklist

- [ ] **Raw data files** in original instrument format
- [ ] **ANALYSIS files** (native search engine/tool outputs)
- [ ] **STANDARD file formats** (mzIdentML/mzTab) when available
- [ ] **Peak list files** (MGF, etc.) if applicable
- [ ] **Parameter files** for all search engines/tools used
- [ ] **FASTA database files** or precise references to public databases
- [ ] **Spectral libraries** if used in the analysis
- [ ] **Complete metadata** (SDRF file)
- [ ] **QC metrics** (if available)
- [ ] **README file** explaining file organization (recommended)

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
