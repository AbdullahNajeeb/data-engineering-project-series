
PROJECT 6: Object-Oriented Data Processing Framework
================================================================================

**DIFFICULTY:** Intermediate-> Advanced

--------------------------------------------------------------------------------

BUSINESS SCENARIO:

  As pipelines grow more complex, functional scripts become hard to maintain.
  Object-oriented design allows data engineers to build modular, reusable
  pipeline components - a reader, a transformer, a writer - that can be
  combined and extended. This project refactors earlier project logic into a
  simple OOP-based processing framework.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Design a base class hierarchy: DataSource, DataTransformer, DataSink.
  2. Implement concrete classes for CSV, JSON, and Parquet reading/writing.
  3. Implement transformer classes for cleaning, filtering, and aggregating.
  4. Chain components together to form a simple pipeline.
  5. Use class-level logging, error handling, and basic metadata tracking.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - Object-Oriented Programming: classes, __init__, methods, attributes
  - Inheritance and method overriding
  - Abstract base classes (abc module) for interface enforcement
  - Encapsulation and separation of concerns
  - Class-based logging (logger per class)
  - Chaining objects together (pipeline pattern - no frameworks)
  - Exception handling inside class methods
  - Dataclasses (optional - for representing records or config)

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Define abstract base classes:
       - DataSource    : has read() method -> returns list of dicts
       - DataTransformer: has transform(data) method -> returns list of dicts
       - DataSink      : has write(data) method -> writes output
  2. Implement concrete subclasses:
       - CSVSource(DataSource)      : reads CSV
       - JSONSource(DataSource)     : reads JSON
       - CSVSink(DataSink)          : writes CSV
       - ParquetSink(DataSink)      : writes Parquet
  3. Implement transformer subclasses:
       - NullHandler(DataTransformer)    : fill or drop nulls
       - DuplicateRemover(DataTransformer): remove duplicate rows
       - ColumnFilter(DataTransformer)   : keep only specified columns
       - TypeCaster(DataTransformer)     : cast columns to target types
  4. Implement a Pipeline class that:
       - Accepts a source, list of transformers, and a sink
       - Calls read -> transform (in order) -> write
       - Logs each stage: start, end, row counts
       - Tracks metadata: pipeline name, start time, end time, rows in, rows out
  5. The pipeline must be configurable via a simple Python dict or config object
     - not hardcoded in the Pipeline class itself.
  6. All classes must handle exceptions internally and log errors without crashing.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS

**Format**: CSV or JSON (learner's choice - framework must support both)

  Required Columns:

    - One unique identifier
    - One date column
    - At least two categorical columns
    - At least two numeric columns

  Volume:

    - Minimum 2,000 rows

  Data Quality Conditions:

    - Null values in at least 3 columns
    - Duplicate rows (for DuplicateRemover to demonstrate effect)
    - Mixed data types in at least one numeric column

  Note: This project reuses cleaned data from Projects 1–2 or introduces a
  new raw dataset matching the schema above.

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES

  - sources.py        : DataSource and subclasses
  - transformers.py   : DataTransformer and subclasses
  - sinks.py          : DataSink and subclasses
  - pipeline.py       : Pipeline class
  - main.py           : Example pipeline configuration and execution
  - output/output_final.csv (or .parquet)
  - README.md

--------------------------------------------------------------------------------

SAMPLE PIPELINE CONFIGURATION (Python Dict)
  pipeline_config = 
 
  {

    "name": "daily_data_clean",
    "source": {"type": "CSV", "path": "data/input.csv"},
    "transformers": [
      {"type": "DuplicateRemover"},
      {"type": "NullHandler", "strategy": {"metric_1": "mean", "label": "drop"}},
      {"type": "TypeCaster", "column_types": {"metric_1": "float", "date": "date"}}
    ],
    "sink": {"type": "Parquet", "path": "output/output_final.parquet"}
  }

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_06_oop_framework/

  ├── data/
  │   └── input.csv

  ├── output/
  │   └── output_final.parquet

  ├── sources.py

  ├── transformers.py

  ├── sinks.py

  ├── pipeline.py

  ├── main.py
  
  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Design and implement a modular OOP pipeline in Python
  - Understand the role of abstract classes in enforcing interfaces
  - Build reusable components that work with any data source/format
  - Track pipeline metadata programmatically (rows in, rows out, timing)

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - What is inheritance and when do you use it in Python?
  - What is an abstract class and what problem does it solve?
  - How would you design a modular pipeline in Python?
  - How do tools like Apache Spark or Airflow relate to this pattern?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  12–15 hours

PREREQUISITES
  Projects 1–5

--------------------------------------------------------------------------------

STRETCH GOALS
  - Add a branching transformer that routes rows to different sinks based on a rule
  - Serialize and deserialize the pipeline config to/from a JSON file
  - Add a dry-run mode that validates the pipeline config without executing it
  - Write unit tests for each transformer class using Python's unittest module