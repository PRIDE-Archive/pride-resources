# PRIDE Database Submission Guidelines
## File Formats and Software Tool Requirements

## Table of Contents
1. [Introduction](#introduction)
2. [Overview of PRIDE Database](#overview-of-pride-database)
3. [Supported Raw Data Formats](#supported-raw-data-formats)
4. [Search Engine and Analysis Tool Requirements](#search-engine-and-analysis-tool-requirements)
   - [Mascot](#mascot)
   - [MaxQuant](#maxquant)
   - [DIA-NN](#dia-nn)
   - [Proteome Discoverer](#proteome-discoverer)
   - [MS-GF+](#ms-gf)
   - [Skyline](#skyline)
   - [FragPipe/MSFragger](#fragpipe-msfragger)
   - [OpenMS](#openms)
   - [Other Tools](#other-tools)
5. [Metadata Requirements](#metadata-requirements)
6. [Best Practices for Submission](#best-practices-for-submission)
7. [File Compression Strategies](#file-compression-strategies)
8. [Submission Checklist](#submission-checklist)
9. [Contact and Support](#contact-and-support)

## Introduction

This document provides comprehensive guidelines for submitting proteomics data to the PRIDE (PRoteomics IDEntifications) database, focusing on the supported file formats and specific requirements for different search engines and analysis tools. Following these guidelines will ensure your data is properly documented, can be effectively reused by the research community, and complies with journal data availability requirements.

## Overview of PRIDE Database

PRIDE is a centralized repository for mass spectrometry-based proteomics data, including raw data, processed results, and associated metadata. It is part of the ProteomeXchange (PX) Consortium, which provides a coordinated submission process to proteomics repositories. PRIDE focuses on enabling data reuse, reproducibility, and transparency in proteomics research.

## Supported Raw Data Formats

Raw data files contain the unprocessed data directly from the mass spectrometer. PRIDE accepts:

| Format | Extension | Vendor/Source | Description |
|--------|-----------|---------------|-------------|
| Thermo RAW | .raw | Thermo Fisher Scientific | Binary format for Thermo instruments |
| Bruker BAF/TDF | .baf, .tdf | Bruker | Format for Bruker instruments |
| SCIEX WIFF | .wiff, .wiff2 | SCIEX | Format for SCIEX instruments |
| Agilent D | .d folder | Agilent | Format for Agilent instruments |
| Waters RAW | .raw | Waters | Format for Waters instruments |
| mzML | .mzml | PSI Standard | Open XML-based standard format |
| mzXML | .mzxml | ISB Standard | Legacy XML format |

**Note:** While vendor-specific formats are accepted, it is recommended to also provide an open standard format (mzML) when possible.

## Search Engine and Analysis Tool Requirements

The following sections detail the specific file formats and requirements for each major proteomics analysis tool and search engine. For each tool, we list the required files, recommended additional files, and important notes to ensure your submission is complete and reusable.

### Mascot

**Required Files:**
- Raw data files (.raw, .mzML)
- Mascot DAT files (.dat)
- Results exported as mzIdentML (.mzid) **strongly recommended**
- Parameter file (.xml)

**Recommended Additional Files:**
- MGF files used for the search
- FASTA database used (or clear reference to public database version)
- Custom modifications list if applicable

**Notes:**
- Include the version of Mascot Server used
- Specify fixed and variable modifications
- Document all search parameters

### MaxQuant

**Required Files:**
- Raw data files (.raw)
- MaxQuant output folder containing:
  - evidence.txt
  - peptides.txt
  - proteinGroups.txt
  - parameters.txt
  - summary.txt
- mqpar.xml (parameter file)

**Recommended Additional Files:**
- msms.txt
- Modified peptides.txt
- mzIdentML export if available

**Notes:**
- Include MaxQuant version used
- FASTA database used for the search (or clear reference to public database version)
- Document LFQ settings if applicable
- Special parameters for match-between-runs or other advanced features

### DIA-NN

**Required Files:**
- Raw data files (.raw, .mzML or instrument-specific formats)
- DIA-NN report:
  - report.tsv (or report.pr_matrix.tsv)
  - report.pg_matrix.tsv
- Parameter configuration file

**Recommended Additional Files:**
- Spectral library used (if applicable)
- FASTA database used (if direct database search)
- Log file from analysis

**Notes:**
- Include DIA-NN version
- Document extraction and quantification settings
- Window scheme used for acquisition

### Proteome Discoverer

**Required Files:**
- Raw data files (.raw)
- Proteome Discoverer result file (.pdresult)
- Exported files:
  - PSMs export (.txt)
  - Peptides export (.txt)
  - Proteins export (.txt)
- mzIdentML export (.mzid) **strongly recommended**

**Recommended Additional Files:**
- Processing and consensus workflows (.pdProcessingWF, .pdConsensusWF)
- FASTA database used (or clear reference)

**Notes:**
- Include Proteome Discoverer version
- Document which nodes and search engines were used in the workflow
- Note scoring thresholds and FDR settings

### MS-GF+

**Required Files:**
- Raw data files (.raw, .mzML)
- Result files (.mzid preferred, or .tsv)
- Parameter file

**Recommended Additional Files:**
- FASTA database used (or clear reference)
- Converted peak lists if used

**Notes:**
- Include MS-GF+ version
- Document all search parameters including fragmentation method
- Note any pre-processing steps

### Skyline

**Required Files:**
- Raw data files (.raw, .mzML or vendor formats)
- Skyline document (.sky)
- Skyline data file (.skyd)
- Exported reports (.csv or .tsv)

**Recommended Additional Files:**
- Spectral library used (.blib)
- Acquisition method (.method) if relevant

**Notes:**
- Include Skyline version used
- Document which Skyline plugins were used
- Describe peak integration settings
- Note interference removal techniques if applied

### FragPipe/MSFragger

**Required Files:**
- Raw data files (.raw, .mzML)
- pepXML files (.pep.xml)
- Protein inference results (.tsv or .txt)
- FragPipe parameter files

**Recommended Additional Files:**
- mzIdentML export if available
- FASTA database used
- PTM Shepherd output if used
- IonQuant output if used

**Notes:**
- Include MSFragger and FragPipe versions
- Document all search parameters, especially for open searches
- Note FDR calculation method

### OpenMS

**Required Files:**
- Raw data files (.raw, .mzML)
- Analysis results in idXML (.idxml)
- Quantification results (if applicable)
- TOPP workflow or KNIME workflow description

**Recommended Additional Files:**
- mzIdentML or mzTab export
- Parameter files for individual TOPP tools

**Notes:**
- Include OpenMS version
- Document which TOPP tools were used
- Describe workflow parameters

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
2. **Standard Formats**: When possible, provide data in both native formats and standard formats (mzML, mzIdentML, mzTab).
3. **Complete Documentation**: Ensure all parameters used in data processing are documented either in parameter files or metadata.
4. **Quality Control**: Include QC metrics when available.
5. **Consistent Naming**: Use consistent, descriptive file naming conventions.
6. **Version Information**: Document software versions for all tools used.
7. **File Compression**: Use appropriate compression strategies to reduce storage requirements (see dedicated section below).

## Submission Checklist

- [ ] Raw data files in original format
- [ ] Processed data files (mzML/MGF/etc.)
- [ ] Identification results files
- [ ] Quantification results files (if applicable)
- [ ] Parameter files for all search engines/tools used
- [ ] FASTA database files or precise references
- [ ] Complete metadata
- [ ] QC metrics (if available)
- [ ] README file explaining file organization (recommended)

## Contact and Support

For specific questions about file formats or submission requirements, contact the PRIDE team:
- Email: pride-support@ebi.ac.uk
- Website: https://www.ebi.ac.uk/pride/
- Submission Tool: PRIDE Submission Tool (https://www.ebi.ac.uk/pride/markdownpage/pridesubmissiontool)

## File Compression Strategies

Proper file compression is essential for efficient data storage and transmission when submitting to PRIDE. Below are recommended compression strategies for different file types:

### Raw Data Files

| File Type | Recommended Compression | Notes |
|-----------|-------------------------|-------|
| Thermo RAW | No compression (maintain as .raw) | These are already in a compressed binary format |
| Agilent .d folders | ZIP or TAR.GZ the entire folder | `tar -czf experiment1.d.tar.gz experiment1.d/` |
| Bruker .d folders | ZIP or TAR.GZ the entire folder | Include all subfolders and files |
| Waters RAW | No compression (maintain as .raw) | These are already in a compressed format |
| SCIEX WIFF | No compression (maintain as .wiff/.wiff2) | These are already efficiently stored |
| mzML/mzXML | GZ compression | `gzip large_file.mzML` to create `large_file.mzML.gz` |

**Important Notes for Raw Data:**
- When compressing directory-based raw data (.d folders), ensure you preserve the directory structure
- PRIDE accepts both uncompressed and compressed raw files
- For very large datasets, consider splitting into multiple compressed archives of reasonable size (< 50GB each)

### Search Results and Analysis Tool Outputs

| Tool | Recommended Compression | Strategy |
|------|-------------------------|----------|
| MaxQuant | ZIP or TAR.GZ the entire output folder | Group all txt files and the andromeda folder into a single archive named `maxquant_results.zip` |
| Mascot | ZIP multiple .dat files together | Group by experiment/project |
| DIA-NN | ZIP all TSV/CSV report files | Include all matrices and report files in one archive |
| Skyline | No compression for .sky/.skyd | These are already in a compressed format |
| FragPipe | ZIP all output directories | Group by experiment/search |
| TPP | ZIP all output XML files | Combine pepXML, protXML and other outputs |

**Example Commands:**
```bash
# Compressing a MaxQuant output directory
zip -r maxquant_results.zip ./maxquant/txt/

# Compressing multiple Mascot DAT files
zip mascot_results.zip *.dat

# Compressing DIA-NN outputs
zip diann_results.zip *.tsv *.csv report.*

# Compressing Agilent .d folder
tar -czf sample1.d.tar.gz ./sample1.d/
```

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
