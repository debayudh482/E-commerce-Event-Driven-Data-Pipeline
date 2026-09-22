### Screenshots

The `screenshots/` directory contains all the screenshots related to the project, including pipeline execution, notebook processing, data ingestion, validation, and other important stages of the E-Commerce Data-Driven Pipeline.

### Initial Pipeline Run — Known Issue

During the **first execution of the E-Commerce Data-Driven Pipeline**, the pipeline required multiple attempts due to a concurrency issue with the `processing_log` table.

The pipeline consists of **5 ingestion processes running in parallel**. Each notebook independently checks for and creates the `processing_log` table before recording its processing status. Since the notebooks execute concurrently during the initial run, multiple notebooks may attempt to create the same table at the same time.

This resulted in an error similar to:

```text
Table `processing_log` already exists
```

The issue occurred because the table creation operation was being performed concurrently by multiple ingestion notebooks.

After the `processing_log` table was successfully created, subsequent pipeline executions were able to access and update the existing table, allowing the ingestion processes to run successfully.

**Root Cause:** Concurrent table creation by multiple parallel ingestion notebooks during the initial pipeline execution.

**Impact:** The initial pipeline execution required multiple attempts.

**Subsequent Runs:** Once the `processing_log` table existed, the pipeline could use the existing table for processing-status tracking without encountering the initial table-creation conflict.
