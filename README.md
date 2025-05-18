Got it! Here's the plain, clean README.md text in one block without emojis or special formatting blocks, ready to copy:

---

# ADF CP Area Config Sync

This repository contains Azure Data Factory (ADF) assets to sync Area Config Master data from the OLTP system to the OLAP data warehouse with proper data quality checks and change detection logic.

Project Overview:

This ADF project ensures accurate and clean data synchronization from \[CP\_OLTP].\[CP\_Area\_Config\_Master] to \[CP\_OLAP].\[CP\_Area\_Dimension], applying trimming, filtering, and SCD logic (insert/update detection).

Key Features:

* Filters only Status = 'Approved' rows
* Trims all text fields to remove extra spaces
* Identifies inserts vs updates using Business\_Unit\_ID
* Adds metadata like Current\_Rec\_In, Begin\_Date, End\_Date
* Reusable and parameterized pipeline

Structure:

/adf

* pipelines/

  * CP\_pl\_areaconfig.json
* dataflows/

  * cp\_df\_areaconfig.dfd.json
* linkedServices/

  * AzureSql\_OLTP.json
  * AzureSql\_OLAP.json
* datasets/

  * CP\_OLTP\_Area\_Config\_Master.json
  * CP\_OLAP\_Area\_Dimension.json

Getting Started:

Prerequisites:

* Azure Subscription
* Azure Data Factory V2
* Git integration enabled in ADF

Setup:

1. Clone this repo:
   git clone [https://github.com/your-org/adf-cp-area-config-sync.git](https://github.com/your-org/adf-cp-area-config-sync.git)
2. Connect Git in ADF:

   * Go to Manage > Git Configuration
   * Use your repo, branch, and root folder
   * Sync your workspace
3. Deploy and publish changes

Dataflow Logic – cp\_df\_areaconfig:

* Source: \[CP\_OLTP].\[CP\_Area\_Config\_Master]
* Filter: Status = 'Approved'
* Trim Columns: Removes leading/trailing spaces
* Join: Match with \[CP\_OLAP].\[CP\_Area\_Dimension] on Business\_Unit\_ID
* Detect Changes:

  * New records → Current\_Rec\_In = 1
  * Updated records → Current\_Rec\_In = 2
* Sink: Append data to dimension table with deduplication

Pipeline – CP\_pl\_areaconfig:

* Invokes the cp\_df\_areaconfig Dataflow
* Can be scheduled or triggered manually
* Supports environment-based parameterization if needed

Notes:

* Destination does not store the Status column.
* SCD logic is based on changes to business fields (not surrogate key).
* Business\_Unit\_ID acts as a unique identifier.
* Uses effective date range logic for tracking changes.

Contact:

Developer: Vamsi
Email: [vamsiroyal07@outlook.com](mailto:vamsiroyal07@outlook.com)

License:

This project is internal-use only. Reach out for external usage rights or licensing queries.

---

Just let me know if you want me to adjust it further!
