
PROJECT 5: Config-Driven Data Validation Framework
================================================================================

**DIFFICULTY**: Intermediate

--------------------------------------------------------------------------------

BUSINESS SCENARIO:
  Data engineers must ensure data quality before loading into target systems.
  Hardcoding validation rules is not maintainable,, a config-driven validation
  framework allows rules to be updated without changing code. This project
  builds a reusable validation layer driven entirely by an external config file.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Load validation rules from a JSON or YAML configuration file.
  2. Apply rules to a dataset (CSV or JSON input).
  3. Support multiple rule types: not-null, data type, range, allowed values,
     format (regex-based), referential integrity (check against a lookup list).
  4. Tag each row as VALID or INVALID with a reason code.
  5. Write valid rows to one file, invalid rows to another.
  6. Produce a validation summary report.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - JSON / YAML config file loading
  - Dictionary-driven logic (rule dispatch pattern)
  - Regular expressions for format validation
  - Functions and higher-order functions (calling functions from a dict)
  - Exception handling
  - Logging (structured log messages)
  - File I/O (CSV read/write)
  - Sets and lists for allowed-value lookups
  - Modular code structure (separate validator module)

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Config file must define rules per column. Supported rule types:
       - not_null          : Column must not be null/empty
       - data_type         : Value must be castable to int/float/date/string
       - min_value         : Numeric value must be >= threshold
       - max_value         : Numeric value must be <= threshold
       - allowed_values    : Value must be in a defined list
       - regex_format      : Value must match a regex pattern
  2. Load the config and dataset at runtime (paths passed as variables).
  3. For each row, apply all applicable rules per column.
  4. Tag each row: status = "VALID" or "INVALID", failed_rules = list of rule names.
  5. Write valid rows to: output_valid.csv
  6. Write invalid rows to: output_invalid.csv (include status and failed_rules columns)
  7. Produce a summary report (printed and written to file):
       - Total rows processed
       - Total valid / invalid
       - Per-rule failure counts
  8. Adding a new rule type should require only a new function + config entry,
     not changes to core processing logic.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS

  **Format**: CSV

  Required Columns:

    - One unique identifier column (not-null rule must apply)
    - One date column (format validation rule must apply)
    - At least two categorical columns (allowed_values rule must apply)
    - At least two numeric columns (min/max range rules must apply)

  Volume:

    - Minimum 2,000 rows

  Data Quality Conditions (intentional — for validation to catch):

    - Null values in at least 3 columns
    - Out-of-range numeric values (e.g., negative quantity, age > 120)
    - Invalid categorical values (e.g., "TypeZ" when only TypeA/TypeB/TypeC allowed)
    - Malformed dates (e.g., "2024-13-45", "not-a-date")
    - At least 10% rows expected to fail at least one rule

  Example Config (JSON format):

    {
      "rules": {
        "id":       [{"type": "not_null"}],
        "date":     [{"type": "not_null"}, {"type": "regex_format", "pattern": "\\d{4}-\\d{2}-\\d{2}"}],
        "category": [{"type": "allowed_values", "values": ["TypeA", "TypeB", "TypeC"]}],
        "amount":   [{"type": "data_type", "target": "float"}, {"type": "min_value", "value": 0}]
      }
    }

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES

  - validator.py          : Core validation engine (rule dispatch logic)
  - rules.py              : Individual rule functions
  - config/rules.json     : Validation rules config file
  - output/output_valid.csv
  - output/output_invalid.csv
  - output/validation_report.txt
  - README.md

--------------------------------------------------------------------------------

SAMPLE INPUT / OUTPUT (Generic Format)

  Input row:

    id=1001, date="2024-13-45", category="TypeZ", amount=-500

  Output (invalid row):

    id=1001, ... , status=INVALID, failed_rules=["date.regex_format", "category.allowed_values", "amount.min_value"]

  Validation Report:

    Total rows         : 2000
    Valid rows         : 1734
    Invalid rows       : 266
    not_null failures  : 45
    allowed_values     : 120
    min_value          : 80
    regex_format       : 21

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_05_validator/

  ├── config/
  │   └── rules.json

  ├── data/
  │   └── input.csv

  ├── output/

  │   ├── output_valid.csv
  │   ├── output_invalid.csv
  │   └── validation_report.txt

  ├── validator.py

  ├── rules.py

  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Design config-driven processing logic (rules outside code)
  - Build an extensible validator without modifying core logic for new rules
  - Apply regex for format validation in a pipeline context
  - Understand data quality gating before downstream load

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - What is config-driven design and why is it important in data pipelines?
  - How do you validate data before loading it to a database?
  - What is the difference between data type validation and range validation?
  - How would you extend a validation framework to support a new rule type?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  10–12 hours

PREREQUISITES
  Projects 1–4

--------------------------------------------------------------------------------

STRETCH GOALS
  - Support YAML config files in addition to JSON
  - Add a cross-column rule: validate that column A < column B
  - Add severity levels to rules: ERROR (reject row) vs WARNING (flag only)
  - Output a per-column quality score (% passing) in the report