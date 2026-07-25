PROJECT 3: Log File Analyzer Using Regular Expressions
================================================================================

**DIFFICULTY** Intermediate

--------------------------------------------------------------------------------

BUSINESS SCENARIO: Data pipelines and systems generate log files continuously. A data engineer
  must be able to parse these logs to identify errors, extract structured fields
  from unstructured text, and produce summaries that help monitor pipeline
  health. This project simulates log analysis using Python regular expressions.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Read a plain-text log file line by line.
  2. Use regular expressions to extract structured fields from each log line.
  3. Categorize log entries by severity level (INFO, WARN, ERROR, DEBUG).
  4. Identify and extract error messages and associated timestamps.
  5. Count events by severity and by time window (e.g., per hour).
  6. Write a structured summary report as CSV.
  7. Flag and save all ERROR-level lines to a separate error log file.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - Regular expressions (re module): match, search, findall, groups, patterns
  - String parsing and pattern matching
  - File reading line by line (memory-efficient)
  - Dictionaries for frequency counting
  - Datetime parsing from strings
  - Functions and modular design
  - Exception handling (malformed lines)
  - Writing output files (CSV and plain text)
  - Python logging module

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Read the log file line by line (do not load the entire file into memory).
  2. Define a regex pattern that captures from each line:
       - Timestamp
       - Log level (INFO, WARN, ERROR, DEBUG)
       - Source component or module name
       - Message body
  3. For lines that do not match the pattern, log a warning and skip them.
  4. Build a frequency table:
       - Count of each log level
       - Count of events per hour (extracted from timestamp)
  5. Extract all ERROR lines and write them to: errors.log
  6. Write a structured summary to: log_summary.csv with columns:
       - timestamp, level, component, message
  7. Print a final console summary:
       - Total lines processed
       - Total valid / invalid lines
       - Counts per log level

--------------------------------------------------------------------------------

DATASET REQUIREMENTS
  Format: Plain text (.log or .txt)

  Required Structure (each log line must follow a pattern, e.g.):
    [TIMESTAMP] [LEVEL] [COMPONENT] MESSAGE

  Example log line patterns:

    2024-01-15 10:23:45 | INFO  | DataLoader   | File loaded successfully
    2024-01-15 10:24:01 | ERROR | Transformer  | Null value found in column: amount
    2024-01-15 10:24:30 | WARN  | Validator    | Record count mismatch: expected 1000, got 987

  Required Characteristics:

    - At least 4 distinct log levels: INFO, WARN, ERROR, DEBUG
    - At least 3 distinct component/module names
    - At least 15% ERROR-level lines
    - At least 5% malformed lines (lines that don't match the expected pattern)
    - Timestamps spanning at least 3 hours

  Volume:
    - Minimum 2,000 log lines

  Note: Learners may generate synthetic log files using a Python script
  or use any application log file that has a consistent line structure.

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES

  - analyzer.py       : All log parsing logic in functions
  - patterns.py       : Regex patterns defined as named constants
  - output/errors.log : Extracted ERROR-level entries
  - output/log_summary.csv : Structured parsed output
  - README.md

--------------------------------------------------------------------------------

SAMPLE INPUT / OUTPUT (Generic Format)

  Input lines:

    2024-01-15 10:00:01 | INFO  | Loader    | Starting data load
    2024-01-15 10:00:05 | ERROR | Validator | Null detected in field: user_id
    MALFORMED LINE WITH NO PATTERN

  Console Summary Output:

    Total lines processed : 2000
    Valid lines           : 1905
    Invalid / skipped     : 95
    INFO count            : 950
    WARN count            : 380
    ERROR count           : 325
    DEBUG count           : 250

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_03_log_analyzer/

  ├── data/
  │   └── application.log

  ├── output/
  │   ├── errors.log
  │   └── log_summary.csv

  ├── analyzer.py

  ├── patterns.py

  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Write and apply regular expressions for structured text extraction
  - Process large files line by line without loading into memory
  - Build frequency and time-window summaries from unstructured data
  - Understand the role of log analysis in pipeline monitoring

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - What is a regular expression and when do you use one in data engineering?
  - How do you process a large file without running out of memory?
  - How would you monitor a data pipeline using logs?
  - What are common log severity levels and what do they indicate?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  8–10 hours

PREREQUISITES
  Projects 1 and 2

--------------------------------------------------------------------------------

STRETCH GOALS
  - Add detection of repeated errors within a time window (error spike detection)
  - Generate a per-hour error rate trend as a simple text-based chart
  - Support multiple log format patterns (configurable regex list)
  - Archive old log entries beyond a certain age into a separate file