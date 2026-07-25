PROJECT 4: JSON to Structured Data Converter
================================================================================

**DIFFICULTY**: Intermediate

--------------------------------------------------------------------------------

BUSINESS SCENARIO:
  APIs, event streams, and modern systems commonly deliver data in JSON format,
  often with nested structures. A data engineer must be able to flatten, parse,
  and convert this semi-structured data into tabular formats suitable for
  downstream processing, databases, or analytics. This project builds that
  conversion layer.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Read JSON data from a file (single object, array of objects, or newline-
     delimited JSON).
  2. Inspect and understand the nested structure.
  3. Flatten nested fields into tabular columns.
  4. Handle missing or null fields gracefully.
  5. Convert the flattened data to a structured CSV and Parquet file.
  6. Log conversion statistics and any parsing errors.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - JSON parsing (json module)
  - Nested dictionary and list traversal
  - Flattening algorithms (recursive or iterative)
  - Handling optional / missing keys safely (dict.get())
  - Type inference and type casting
  - Writing CSV and Parquet files (pyarrow or pandas)
  - Exception handling for malformed JSON records
  - Logging
  - Virtual environments and package installation (venv, pip)

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Accept input as:
       - A JSON array file (list of objects)
       - A newline-delimited JSON file (one JSON object per line)
       - Detect format automatically or via a config flag.
  2. Inspect and print the schema (top-level keys, nested keys, data types).
  3. Flatten nested objects using a separator (default: double underscore "__"):
       Example: {"address": {"city": "X"}} -> address__city: "X"
  4. Flatten nested arrays: convert the first-level list to pipe-separated string
       or create one row per array element (configurable).
  5. Handle missing keys: fill with None, do not crash.
  6. Write flattened output to:
       - output_flat.csv
       - output_flat.parquet
  7. Log per-record parsing stats: total records, successful, failed/skipped.
  8. All logic in functions; no logic in global scope.
  9. Use a virtual environment; include requirements.txt.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS"
  Format: JSON (array or newline-delimited)

  Required Structure:

    - A unique identifier field at the top level
    - At least one timestamp or date field
    - At least two categorical fields (top-level)
    - At least two numeric fields (top-level or nested)
    - At least one nested object (one level deep)
    - At least one array field (list of values or list of objects)
    - Optional fields that are missing in some records (to test null handling)

  Volume:
    - Minimum 1,000 JSON records

  Data Quality Conditions:
    - At least 5% records with a missing nested key
    - At least 3% malformed or null records (empty object, null, missing id)
    - At least one array field with variable-length arrays across records

  Example Schema (generic):

    {
      "id": "string",
      "event_timestamp": "string",
      "category": "string",
      "status": "string",
      "metrics": {
        "value_a": number,
        "value_b": number
      },
      "tags": ["string", "string"],
      "sub_category": "string"     <- may be missing in some records
    }

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES
  - converter.py          : Main conversion logic
  - flattener.py          : Recursive / iterative flatten utility
  - config.py             : Input format flag, separator config
  - output/output_flat.csv
  - output/output_flat.parquet
  - requirements.txt
  - README.md

--------------------------------------------------------------------------------

SAMPLE INPUT / OUTPUT (Generic Format)

  Input record:

    {
      "id": "REC001",
      "event_timestamp": "2024-01-15T10:30:00",
      "category": "TypeA",
      "metrics": {"value_a": 1500, "value_b": 200},
      "tags": ["urgent", "reviewed"]
    }

  Flattened output row:

    id        | event_timestamp      | category | metrics__value_a | metrics__value_b | tags
    REC001    | 2024-01-15T10:30:00  | TypeA    | 1500             | 200              | urgent|reviewed

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_04_json_converter/

  ├── data/
  │   └── records.json
  
  ├── output/
  │   ├── output_flat.csv
  │   └── output_flat.parquet

  ├── converter.py

  ├── flattener.py

  ├── config.py

  ├── requirements.txt

  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Parse and traverse nested JSON structures in Python
  - Build a reusable flattening utility applicable to any JSON schema
  - Write Parquet files using pyarrow or pandas
  - Handle optional and missing fields without crashing
  - Set up and use Python virtual environments

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - What is newline-delimited JSON and when is it used?
  - How do you flatten a nested JSON structure?
  - What is Parquet and why is it preferred over CSV in data engineering?
  - How do you safely access a key that might not exist in a Python dictionary?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  8–10 hours

PREREQUISITES
  Projects 1, 2, 3

--------------------------------------------------------------------------------

STRETCH GOALS
  - Support two-level deep nesting flattening
  - Auto-detect and infer Parquet column data types
  - Add schema validation: reject records that don't contain required keys
  - Build a schema introspection function that prints the full nested structure