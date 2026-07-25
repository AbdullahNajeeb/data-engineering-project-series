PROJECT 1: Simple CSV Data Processor
================================================================================


**DIFFICULTY**
  Basic

--------------------------------------------------------------------------------

**BUSINESS** **SCENARIO**
  Organizations routinely receive flat files (CSVs) from upstream systems,
  partners, or exports. Before any processing or analysis can happen, a data
  engineer must be able to read, inspect, filter, aggregate, and write these
  files reliably using Python. This project simulates that first-touch
  processing step.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Read a CSV file into memory using Python's built-in csv module.
  2. Inspect the structure, column names, and basic statistics.
  3. Filter rows based on simple conditions.
  4. Perform basic aggregations (counts, sums, averages) per category.
  5. Write the processed output to a new CSV file.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - Variables, data types, type casting
  - Operators and conditional execution
  - Loops and iterations
  - Functions (defining and calling)
  - String operations and formatting
  - Basic file I/O (open, read, write, close)
  - Python's built-in csv module
  - Lists and basic list operations
  - print() for output formatting

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Read a CSV file from a configurable file path (pass path as a variable).
  2. Print a summary:
       - Total row count
       - Column names
       - Data types (inferred from values)
  3. Filter rows where a numeric column exceeds a threshold (hardcoded is fine).
  4. Group rows by one categorical column and calculate:
       - Count of rows per group
       - Sum of a numeric column per group
       - Average of a numeric column per group
  5. Write filtered rows to a new file: output_filtered.csv
  6. Write aggregated results to a new file: output_summary.csv
  7. Handle the case where the input file does not exist (print a clear error).
  8. All logic must be organized into functions — no logic in global scope.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS
  Format: CSV

  Required Columns:

    - One unique identifier column (e.g., id, record_id)
    - One date or timestamp column (e.g., date, created_at)
    - At least two categorical columns (e.g., category, region, status, type)
    - At least two numeric columns (e.g., amount, quantity, score, duration)

  Volume:

    - Minimum 500 rows

  Data Quality Conditions (must be present):

    - At least 5% rows with null values in any non-key column
    - At least 10 duplicate rows (same record appearing twice)
    - Mixed casing in at least one categorical column (e.g., "Active", "active", "ACTIVE")
    - At least one numeric column with values stored as strings (e.g., "123.45")

  Example Schema (generic):

    id            | string   | Unique record identifier
    created_date  | string   | Date of the record (YYYY-MM-DD)
    category      | string   | Categorical attribute 1
    region        | string   | Categorical attribute 2
    value_1       | string   | Numeric attribute (may be stored as string)
    value_2       | float    | Numeric attribute
    status        | string   | Categorical attribute 3

  Note: Learners may use any dataset matching this schema — retail transactions,
  employee records, survey results, support tickets, etc.

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES
  - processor.py          : Main script with all functions
  - output_filtered.csv   : Rows passing the filter condition
  - output_summary.csv    : Aggregation results by category
  - README.md             : Brief explanation of what the script does and how to run it

--------------------------------------------------------------------------------

SAMPLE INPUT / OUTPUT (Generic Format)

  Input (first 3 rows):

    id,created_date,category,region,value_1,value_2,status
    1001,2024-01-15,TypeA,North,"1500.00",200,active
    1002,2024-01-16,TypeB,South,"800.00",150,Active
    1002,2024-01-16,TypeB,South,"800.00",150,Active   <- duplicate

  Output — output_summary.csv:
  
    category,record_count,total_value_1,avg_value_1
    TypeA,210,315000.00,1500.00
    TypeB,180,144000.00,800.00

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_01_csv_processor

  ├── data ── input.csv
  
  ├── output
      ── output_filtered.csv
      ── output_summary.csv

  ├── processor.py
  
  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Confidently read and write CSV files using Python
  - Apply filtering and grouping logic without external libraries
  - Organize code into reusable functions
  - Handle basic file errors gracefully

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - How do you read a CSV in Python without pandas?
  - What is the csv.DictReader vs csv.reader difference?
  - How do you handle missing files in Python?
  - What does type casting mean and when do you need it?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  6–8 hours

PREREQUISITES
  None — this is the first project.

--------------------------------------------------------------------------------

STRETCH GOALS
  - Accept the file path and filter threshold as command-line arguments (sys.argv)
  - Support multiple filter conditions combined with AND / OR logic
  - Output a basic data profile (min, max, null count per column)
  - Auto-detect and skip duplicate rows during read