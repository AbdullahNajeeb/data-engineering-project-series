PROJECT 2: Based Data Cleaner and Transformer
================================================================================

DIFFICULTY
  Basic → Intermediate

--------------------------------------------------------------------------------

BUSINESS SCENARIO:
  Raw data arriving from source systems is almost never clean. Null values,
  duplicates, format inconsistencies, and incorrect data types are standard
  problems a data engineer must solve before data can be loaded downstream.
  This project builds a reusable data cleaning and transformation layer.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Load a CSV file and identify data quality issues systematically.
  2. Remove or flag duplicate rows.
  3. Handle null values using configurable strategies (drop, fill, flag).
  4. Standardize categorical column values (case normalization, trimming).
  5. Convert columns to correct data types.
  6. Apply basic transformations (derived columns, string formatting).
  7. Output a clean CSV and a data quality report.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - Functions and modular code design
  - String methods (strip, lower, upper, replace)
  - Type conversion and error handling during conversion
  - Conditional logic for null handling strategies
  - Lists and dictionaries for tracking quality metrics
  - File reading and writing (csv module)
  - Basic exception handling (try / except)
  - Logging (Python logging module - basic usage)

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Load the input CSV and generate a pre-cleaning quality report:
       - Row count
       - Null count per column
       - Duplicate row count
       - Unique value count per categorical column
  2. Implement configurable null handling:
       - For numeric columns: fill with column mean or a fixed default value
       - For categorical columns: fill with "UNKNOWN" or drop the row
       - Strategy must be defined as a dictionary (not hardcoded per column)
  3. Remove exact duplicate rows (all columns identical).
  4. Standardize all string columns: strip whitespace, convert to title case.
  5. Convert columns to their correct data types based on a column type map
     (a dictionary you define mapping column_name - target_type).
  6. Add a derived column: a cleaned/processed timestamp (if a date column
     exists, standardize to YYYY-MM-DD format).
  7. Write the clean output to: output_clean.csv
  8. Write the quality report to: quality_report.txt or quality_report.csv
  9. Log each cleaning step to console using Python's logging module.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS
  Format: CSV

  Required Columns:

    - One unique identifier column
    - One date column with inconsistent formats (e.g., "2024/01/15", "15-01-2024", "Jan 15 2024")
    - At least two categorical columns with inconsistent casing and whitespace
    - At least two numeric columns,  one stored as a string type
    - At least one column with frequent nulls (>10% null rate)

  Volume:

    - Minimum 1,000 rows

  Data Quality Conditions:

    - Null values: present in at least 3 different columns
    - Duplicate rows: at least 50 duplicates
    - Mixed date formats: at least 3 different date format patterns in the same column
    - Mixed casing: at least one categorical column with 3+ case variants
    - Numeric stored as string: at least one column (e.g., "1,500.00" with commas)

  Example Schema (generic):

    record_id     | string   | Unique identifier
    event_date    | string   | Date in inconsistent formats
    type          | string   | Category with mixed casing/whitespace
    sub_type      | string   | Second categorical attribute
    metric_1      | string   | Numeric stored as string (may have commas)
    metric_2      | float    | Numeric, has nulls
    label         | string   | Categorical, has nulls

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES

  - cleaner.py            : All cleaning logic organized into functions
  - config.py             : Null handling strategy and column type maps
  - output/output_clean.csv
  - output/quality_report.csv
  - README.md

--------------------------------------------------------------------------------

SAMPLE INPUT / OUTPUT (Generic Format)

  Input row:

    1001 | "jan 15 2024" | " typeA " | subB | "1,500.00" | null | null

  Output row:

    1001 | 2024-01-15 | Type A | Sub B | 1500.00 | <mean_filled> | UNKNOWN

  Quality Report (text):

    Total rows read        : 1000
    Duplicates removed     : 52
    Nulls filled (metric_2): 110
    Nulls filled (label)   : 85
    Rows written (clean)   : 948

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_02_data_cleaner/

  ├── data/
  │   └── raw_input.csv

  ├── output/
  │   ├── output_clean.csv
  │   └── quality_report.csv

  ├── cleaner.py

  ├── config.py
  
  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Apply systematic data quality checks before processing
  - Build configurable, reusable cleaning logic (not hardcoded per column)
  - Use Python logging for basic pipeline observability
  - Understand the difference between dropping vs. filling null values

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - How do you handle null values in a data pipeline?
  - What are common data quality issues in real-world data?
  - How do you make cleaning logic configurable vs. hardcoded?
  - What is Python's logging module and why use it over print()?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  8–10 hours

PREREQUISITES
  Project 1

--------------------------------------------------------------------------------

STRETCH GOALS
  - Accept the null strategy config as a JSON config file instead of a Python dict
  - Add a column-level data type validation step that flags unexpected values
  - Generate an HTML quality report along with a plain text file
  - Support semicolon or tab-delimited files, not just comma-separated