ADF CP Area Config Sync

This repository contains Azure Data Factory (ADF) assets designed to synchronize Area Config Master data from the OLTP system to the OLAP data warehouse with robust data quality checks, error handling, and change detection logic.

 Project Overview

This ADF project ensures accurate data migration from [CP_OLTP].[CP_Area_Config_Master] to [CP_OLAP].[CP_Area_Dimension], with enhancements for error logging, trimming, filtering, and SCD tracking.

 Key Features

✔ Filters only Approved rows (Status = 'Approved')✔ Trims all text fields to remove extra spaces✔ Detects Inserts & Updates using Business_Unit_ID✔ Implements default values (Begin_Date, End_Date, Current_Rec_In)✔ Tracks data changes dynamically (Current_Rec_In increments for updates)✔ Error Handling Mechanism logs failed records in a separate table✔ Reusable and parameterized pipeline for future scaling

 Repository Structure

/adf
 ├── pipelines/
 │    ├── CP_pl_areaconfig.json
 │ 
 ├── dataflows/
 │    ├── cp_df_areaconfig.dfd.json
 │ 
 ├── linkedServices/
 │    ├── AzureSql_OLTP.json
 │    ├── AzureSql_OLAP.json
 │ 
 ├── datasets/
 │    ├── CP_OLTP_Area_Config_Master.json
 │    ├── CP_OLAP_Area_Dimension.json

 Getting Started

 Prerequisites

 Azure Subscription

 Azure Data Factory V2

 Git integration enabled in ADF

 Setup

 Clone this repo

git clone https://github.com/your-org/adf-cp-area-config-sync.git

 Connect Git in ADF

Go to Manage > Git Configuration

Use your repo, branch, and root folder

Sync your workspace

Deploy and publish changes

Dataflow Logic – cp_df_areaconfig

Step-by-Step Transformation

Source → Reads from [CP_OLTP].[CP_Area_Config_Master]2️⃣ Format String → Cleans text columns (removes spaces)3️⃣ Status Filter → Filters rows (Status = 'Approved')4️⃣ Alter Row → Determines inserts vs updates5️⃣ Set Default Values → Assigns default values:

Begin_Date = currentUTC()

End_Date = toDate('9999-12-31', 'yyyy-MM-dd')

Current_Rec_In = 1 (or increments for updates)6️⃣ Load Destination → Inserts/updates in [CP_OLAP].[CP_Area_Dimension]

Error Handling Mechanism

Failed records are logged in [CP_OLTP].[CP_Error_Log]✔ Stored Procedure captures errors dynamically✔ Pipeline failure detection ensures troubleshooting

Error Log Table

CREATE TABLE [CP_OLTP].[CP_Error_Log] (
    Error_ID INT IDENTITY(1,1) PRIMARY KEY,
    Pipeline_Name NVARCHAR(255),
    Error_Message NVARCHAR(MAX),
    Failed_Record NVARCHAR(MAX),
    Timestamp DATETIME DEFAULT GETDATE()
);

 Automatically logs errors during pipeline execution.

Change Tracking (SCD Type 1)

Initial Insert: Current_Rec_In = 1✔ First Update: Current_Rec_In = 2✔ Further Changes: Increment dynamically (Current_Rec_In + 1)

Logic Implementation in Derived Column:

iif(isNull(Current_Rec_In_Lookup), 1, Current_Rec_In_Lookup + 1)

Contact

Developer: Vamsi, Email: vamsiroyal07@outlook.com

License

This project is internal-use only. Reach out for external usage rights or licensing queries.
