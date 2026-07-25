PROJECT 7: REST API Data Ingestion Pipeline
================================================================================

**DIFFICULTY:** Advanced

--------------------------------------------------------------------------------

BUSINESS SCENARIO:
  Most modern data engineering pipelines ingest data from REST APIs - internal
  services, third-party data providers, or SaaS platforms. This project builds
  a complete API ingestion pipeline: authenticate, paginate, extract data,
  handle errors, and persist the results in both raw and processed form.

--------------------------------------------------------------------------------

OBJECTIVES
  1. Call a REST API endpoint with authentication (API key or Bearer token).
  2. Handle paginated responses to retrieve all available records.
  3. Save raw API responses as JSON files (raw landing zone).
  4. Parse and flatten the API response into a structured tabular format.
  5. Implement retry logic for failed API calls.
  6. Log all API calls with timestamps, status codes, and record counts.
  7. Write processed data to CSV and Parquet.

--------------------------------------------------------------------------------

CONCEPTS COVERED
  - HTTP requests using the requests library
  - Authentication: API key headers, Bearer tokens
  - Pagination: page-based and cursor-based
  - Rate limiting awareness (time.sleep between calls)
  - Retry logic with exponential backoff
  - JSON response parsing and flattening
  - Saving raw responses to disk (raw zone pattern)
  - Logging API call details
  - Virtual environment and requirements.txt
  - Exception handling for HTTP errors (4xx, 5xx)

--------------------------------------------------------------------------------

DETAILED REQUIREMENTS
  1. Define API configuration in a config file or dictionary:
       - base_url, endpoint, auth_type, api_key or token, page_size
  2. Implement a function to call the API with:
       - Correct auth headers
       - Pagination params (page number or cursor)
       - Timeout setting
  3. Implement retry logic:
       - Retry up to 3 times on failure
       - Wait 2^attempt seconds between retries (exponential backoff)
       - Log each retry attempt
  4. For each API page response:
       - Save raw JSON to: raw/response_page_{N}.json
       - Flatten records and append to a master list
  5. After all pages are retrieved:
       - Write flattened records to: processed/output.csv
       - Write flattened records to: processed/output.parquet
  6. Log each API call: URL called, timestamp, status code, records returned.
  7. If a page fails after all retries, log the failure and continue (do not crash).

  NOTE: If a public API is not available, build a mock API server using Python's
  http.server module or Flask that returns paginated fake data.

--------------------------------------------------------------------------------

DATASET REQUIREMENTS
  Format: REST API JSON Response

  Required Response Structure (per record):
    - A unique identifier field
    - A timestamp or date field
    - At least two categorical fields
    - At least two numeric fields
    - At least one nested object or array field

  Volume:
    - Minimum 500 records across at least 5 paginated API calls

  Data Quality Conditions (simulate in mock API or expect from real API):
    - At least one page response with a missing optional field in some records
    - At least one simulated timeout or error response for retry testing

  Suggested Public APIs (if using a real API):
    - Open-Meteo (weather data, no auth required)
    - JSONPlaceholder (fake REST API, no auth required)
    - Any public government data API
    - NASA Open APIs (api.nasa.gov, free key)

--------------------------------------------------------------------------------

EXPECTED DELIVERABLES
  - ingestion.py          : API call, pagination, retry logic
  - parser.py             : Response flattening and type conversion
  - config.py             : API config (base_url, auth, page_size, etc.)
  - mock_api.py           : (if no real API) mock server for local testing
  - raw/                  : Raw JSON response files
  - processed/output.csv
  - processed/output.parquet
  - requirements.txt
  - README.md

--------------------------------------------------------------------------------

SAMPLE API CALL LOG OUTPUT

  [2024-01-15 10:00:01] GET https://api.example.com/data?page=1&page_size=100
  Status: 200 | Records returned: 100 | Duration: 0.45s

  [2024-01-15 10:00:02] GET https://api.example.com/data?page=2&page_size=100
  Status: 500 | Retry 1/3 | Waiting 2s...
  Status: 500 | Retry 2/3 | Waiting 4s...
  Status: 200 | Records returned: 100 | Duration: 0.38s

--------------------------------------------------------------------------------

SUGGESTED FOLDER STRUCTURE

  project_07_api_ingestion/

  ├── raw/
  │   ├── response_page_1.json
  │   └── response_page_2.json

  ├── processed/
  │   ├── output.csv
  │   └── output.parquet

  ├── ingestion.py

  ├── parser.py

  ├── config.py

  ├── mock_api.py

  ├── requirements.txt
  
  └── README.md

--------------------------------------------------------------------------------

LEARNING OUTCOMES
  - Build a complete API ingestion pipeline from scratch
  - Implement retry logic with exponential backoff
  - Understand the raw zone vs processed zone pattern
  - Apply pagination handling for large result sets
  - Log API activity for pipeline monitoring

--------------------------------------------------------------------------------

INTERVIEW TOPICS COVERED
  - How do you handle API rate limiting in a pipeline?
  - What is exponential backoff and why is it used?
  - What is the difference between page-based and cursor-based pagination?
  - How does API ingestion work in tools like Azure Data Factory or AWS Glue?
  - What is a raw landing zone and why is it important?

--------------------------------------------------------------------------------

ESTIMATED COMPLETION TIME
  12–15 hours

PREREQUISITES
  Projects 1–6

--------------------------------------------------------------------------------

STRETCH GOALS
  - Support cursor-based pagination in addition to page-number-based
  - Add a checkpointing mechanism: save the last successful page so a
    failed run can resume from where it stopped
  - Add response schema validation before saving raw files
  - Implement parallel API calls using concurrent.futures for speed