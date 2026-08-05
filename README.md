# Python Forensics Toolkit

## Overview

Python Forensics Toolkit is a collection of Python utilities developed to compare, validate, investigate, and report on very large operational datasets.

These tools were built to answer practical questions such as:

- Is a file fully represented in a trusted reference dataset?
- Which records are redundant?
- Which records remain unique?
- Is a specific section safe to archive or remove?
- How much overlap exists between large data environments?

The toolkit uses record qualification rules, cryptographic fingerprints, SQLite indexing, recursive file processing, and automated CSV reporting to support repeatable forensic analysis.

---

## Current Modules

### `run_C1_forensic_compare.py`

A two-phase forensic redundancy analysis engine for comparing an entire file universe against a qualified reference dataset.

The first phase:

- Recursively scans reference `.txt` and `.csv` files
- Applies configurable qualification rules
- Hashes qualifying records
- Builds a persistent SQLite reference index
- Uses batch inserts and periodic commits for large-scale processing

The second phase:

- Scans the comparison environment
- Compares qualifying records against the indexed reference data
- Assigns file-level verdicts
- Produces detailed and global CSV reports

Possible verdicts include:

- `NO_VALID_DATA`
- `REDUNDANT`
- `UNIQUE_PRESENT`
- `READ_ERROR__KEEP`

The engine includes:

- SQLite WAL mode
- Batch indexing
- Memory and cache tuning
- Progress heartbeats
- Timestamped reports
- Defensive retention logic
- Global redundancy statistics

---

### `run_section_compare.py`

A focused forensic comparison utility for evaluating one selected directory or operational section against the qualified reference dataset.

The target section is supplied through the `SECTION_OVERRIDE` environment variable.

For each file, the script reports:

- Qualifying lines
- Matched records
- Unmatched records
- `SafeToDelete` determination

A file is only marked safe when:

1. It contains qualifying data
2. Every qualifying record already exists in the reference dataset

This supports staged review, controlled cleanup, migration validation, and section-by-section analysis.

---

## Processing Architecture

```text
Reference Dataset
      ↓
Qualification Gate
      ↓
SHA-1 Record Fingerprinting
      ↓
SQLite Reference Index
      ↓
Comparison Dataset or Selected Section
      ↓
Record-Level Lookups
      ↓
File Verdicts
      ↓
CSV Reports and Global Statistics
```

---

## Qualification Logic

Records are screened before comparison.

A record must contain:

- A name-like token
- A numeric address pattern
- A ZIP code or city-like token

Only qualifying records are hashed and compared.

This reduces noise and focuses the analysis on operationally meaningful data.

---

## Output Reports

The full comparison engine generates:

```text
C_REDUNDANCY_FILE_REPORT__YYYYMMDD_HHMMSS.csv
C_REDUNDANCY_GLOBAL_SUMMARY__YYYYMMDD_HHMMSS.csv
qualified_hash_index.sqlite
```

The section comparison utility generates:

```text
C_REDUNDANCY_REPORT__SECTION_NAME.csv
```

---

## Technical Capabilities Demonstrated

- Python
- SQLite
- Recursive file-system traversal
- Regular-expression validation
- Hash-based record comparison
- Batch database inserts
- Long-running process telemetry
- CSV report generation
- Environment-variable configuration
- Operational safety controls
- Large-scale data analysis
- Defensive error handling
- Workflow automation

---

## Design Priorities

### Scale

The tools are designed to process large collections of files and millions of qualifying records.

### Repeatability

The same workflows can be rerun across different datasets or operational sections.

### Safety

Unreadable or uncertain files are not automatically classified as removable.

### Visibility

Progress heartbeats and timestamped reports provide operational transparency during long-running jobs.

### Practicality

These tools were created to solve real data-management and forensic comparison problems rather than serve as academic examples.

---

## Current Limitations

The current public versions contain environment-specific local paths and assume Windows-based directory structures.

Future revisions will include:

- Command-line arguments
- External configuration files
- Improved exception logging
- Automated tests
- Sample datasets
- Packaged installation
- Configurable hashing options
- More detailed validation rules

---

## Security Note

SHA-1 is used here as a fast equality fingerprint for record comparison, not for passwords, authentication, encryption, or security-sensitive storage.

No private datasets are included in this repository.

---

## Author

**Patrick Estrada**

Systems Architect  
Automation Engineer  
AI-Assisted Development  
Mission-Critical Operations
