# PRIDE Submission YAML Validation Definition — Design Spec

**Date:** 2026-04-07  
**Author:** Yasset Perez-Riverol  
**Status:** Draft  

## Goal

Create a single YAML file (`pride-submission-validation.yaml`) that encodes all PRIDE submission validation rules as a machine-readable source of truth. This file replaces the markdown guidelines as the authoritative definition for validation logic, while the markdown can be auto-generated from it.

## Consumers

1. **PRIDE Submission Tool** (Java) — validates submissions at upload time
2. **Python validator scripts** — standalone CLI validation of dataset folders
3. **Documentation generators** — auto-generate markdown/HTML guidelines from the YAML

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| File structure | Single monolithic YAML | Simplest to version, parse, and distribute across Java/Python/doc-gen |
| Style | Machine-optimized, flat | Consumers are code systems, not humans reading raw YAML |
| Tool profiles | Fully self-contained | Each tool block lists ALL required files (RAW, FASTA, SDRF, plus tool-specific). No inheritance to resolve at runtime. |
| Content validation | Column names only (v1) | Pragmatic first version; schema designed so column types and constraints can be added later |
| Conditional rules | Simple flags | Submission declares flags (e.g., `library_search: true`); file entries reference flags via `condition_flag` field |

## Top-Level Structure

```yaml
version: "1.0.0"
description: "PRIDE submission validation rules"

compression:
  supported_formats: [zip, gz, tar.gz]
  rejected_formats: [rar]
  max_archive_size_gb: 50

raw_formats:
  - id: <string>
    extensions: [<string>, ...]
    vendor: <string>
    compression_required: <bool>
    one_run_per_archive: <bool>
    directory_based: <bool>

condition_flags:
  - id: <string>
    description: <string>

file_categories:
  - ANALYSIS
  - RAW
  - PEAK
  - STANDARD
  - SPECTRAL_LIBRARY
  - FASTA
  - METADATA
  - OTHER

tools:
  - <tool_profile>
```

### `raw_formats` entries

Define every accepted raw data format with compression and archive rules:

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier (e.g., `thermo_raw`, `bruker_d`) |
| `extensions` | string[] | Accepted file extensions |
| `vendor` | string | Instrument vendor name |
| `compression_required` | bool | Whether compression is mandatory (true for .d folders) |
| `one_run_per_archive` | bool | Whether each archive must contain exactly one run |
| `directory_based` | bool | Whether the format is a directory (e.g., .d folders) |

Defined formats: `thermo_raw`, `bruker_d`, `sciex_wiff`, `agilent_d`, `waters_raw`, `mzml`.

### `condition_flags` entries

Flags that a submission declares to activate conditional file requirements:

| Flag ID | Description |
|---------|-------------|
| `library_search` | Spectral library was used in the analysis |
| `dia_workflow` | Data-Independent Acquisition workflow |
| `custom_modifications` | Custom modifications were used |
| `quantification_performed` | Quantification analysis was performed |
| `ms3_acquired` | MS3 spectra were acquired |

## Tool Profile Structure

Each tool in the `tools` array is fully self-contained:

```yaml
tools:
  - id: <string>
    name: <string>
    description: <string>
    
    files:
      - name: <string>
        status: mandatory | recommended | optional
        category: <file_category>
        extensions: [<string>, ...]
        description: <string>
        columns: [<string>, ...]        # optional, for content validation
        conditional: <bool>              # optional, default false
        condition_flag: <string>         # required if conditional is true

    compression:
      analysis_grouping: single_archive | per_file | no_compression
      recommended_name: <string>
      strategy: <string>

    cross_file_rules:
      - id: <string>
        rule_type: reference_check | column_reference_check | file_count | archive_content
        description: <string>
        # Additional fields depend on rule_type (see below)
```

### File entry fields

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | yes | File name or logical name (e.g., `evidence.txt`, `raw_files`) |
| `status` | enum | yes | `mandatory`, `recommended`, or `optional` |
| `category` | enum | yes | One of the `file_categories` values |
| `extensions` | string[] | yes | Accepted file extensions |
| `description` | string | yes | Human-readable description |
| `columns` | string[] | no | Expected column names for tabular files |
| `conditional` | bool | no | Whether this file is conditionally required (default: false) |
| `condition_flag` | string | no | Which flag activates this requirement (required if `conditional: true`) |

### Cross-file rule types

#### `reference_check`
Files in one category must be referenced in a target file.

```yaml
- id: raw_sdrf_match
  rule_type: reference_check
  source_category: RAW
  target_file: "sdrf.tsv"
  description: "Each raw file must be referenced in the SDRF file"
```

#### `column_reference_check`
Column values in a file must match submitted filenames.

```yaml
- id: evidence_raw_match
  rule_type: column_reference_check
  source_file: "evidence.txt"
  source_column: "Raw file"
  target_category: RAW
  description: "Raw file column in evidence.txt must match submitted raw filenames"
```

#### `file_count`
Minimum/maximum number of files in a category.

```yaml
- id: raw_file_count
  rule_type: file_count
  category: RAW
  min: 1
  description: "At least one raw file must be submitted"
```

#### `archive_content`
Rules about contents inside compressed archives.

```yaml
- id: d_folder_structure
  rule_type: archive_content
  applies_to_extensions: [.d.zip, .d.tar.gz]
  required_contents:
    - pattern: "*/analysis.tdf"
      vendor: bruker_tdf
    - pattern: "*/AcqData/*"
      vendor: agilent
  description: "Directory-based raw formats must contain expected internal structure"
```

## Tools Included

10 tool profiles, each fully self-contained:

| Tool ID | Name | Key ANALYSIS files |
|---------|------|--------------------|
| `maxquant` | MaxQuant | evidence.txt, peptides.txt, proteinGroups.txt, mzTab.mzTab |
| `diann` | DIA-NN | report.parquet/report.tsv, pr_matrix.tsv, pg_matrix.tsv |
| `spectronaut` | Spectronaut | Main report (.tsv/.csv/.xls), protein/peptide reports |
| `fragpipe` | FragPipe/MSFragger | psm.tsv, protein.tsv, combined_*.tsv |
| `skyline` | Skyline | .sky, .skyd, exported reports (.csv/.tsv) |
| `openms` | OpenMS | .idxml, .consensusXML, .featureXML |
| `mascot` | Mascot | .dat files |
| `proteome_discoverer` | Proteome Discoverer | .pdresult, PSMs/Peptides/Proteins exports |
| `msgfplus` | MS-GF+ | .mzid results |
| `generic` | Other Tools | At least one ANALYSIS file (any format) |

Each profile includes: all common files (RAW, FASTA, SDRF), tool-specific ANALYSIS files with column definitions where applicable, compression strategy, and cross-file validation rules.

## Extensibility

The schema is designed for forward-compatible extension:

- **Column types/constraints**: Add `column_type` and `column_constraints` fields to file entries alongside `columns`
- **New tools**: Add a new entry to the `tools` array — no changes to existing entries needed
- **New rule types**: Add new `rule_type` values to `cross_file_rules`
- **New condition flags**: Add entries to `condition_flags`

## File Location

The YAML file will live at:
```
guidelines/pride-submission-validation.yaml
```

Alongside the existing `pride-submission-formats-guidelines.md`.

## Out of Scope (v1)

- Column data types and value constraints (planned for v2)
- Automated markdown generation script (separate task)
- JSON Schema formal definition of the YAML structure itself (can be added later)
- Validation of SDRF content (covered by separate SDRF validation tools)
