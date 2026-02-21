# SSIS ETL Pipeline: B2B Sales Analysis System (Hardware)

## Project Description
This project processes technology equipment sales data for corporate clients. It utilizes **SQL Server Integration Services (SSIS)** to extract data from flat files, transform it (data cleansing and key matching), and load it into a **Data Warehouse** structured under a Star Schema model in **Microsoft SQL Server**.

## Architecture and Technologies
* **Data Sources (Source):** Flat files (.csv / .txt) containing transactional data and master catalogs.
* **Orchestration and ETL:** Microsoft SQL Server Integration Services (SSIS).
* **Storage (Target):** Microsoft SQL Server (T-SQL).
* **Data Modeling:** Dimensional Model (Star Schema).

## Data Modeling: Star Schema
A dimensional model optimized for Business Intelligence was designed:

* **Dimension Tables (Descriptive Context):**
  * `Dim_Cliente`: Stores purchasing companies, classified by `Segment` (Corporate, SME, Government) and `City` (Lima, Arequipa, Cusco, etc.).
  * `Dim_Producto`: Inventory catalog including `Product_Name` (Laptop, Monitor, etc.), `Category` (Electronics, Accessories), and `Reference_Price`.
* **Fact Table (Transactional Metrics):**
  * `Hechos_Ventas`: Central table at a daily granularity level. It records each transaction with foreign keys (`ID_Producto`, `ID_Cliente`), dates (`Fecha_Venta`), and quantitative metrics (`Cantidad`, `PrecioUnitario`).

## SSIS ETL Pipeline (Workflow)
The ETL process ensures data integrity through the following steps:

### 1. Control Flow
A sequential load was implemented to maintain the database's referential integrity:
* Execution of the `Carga Dim_Producto` and `Carga Dim_Cliente` tasks to ensure master catalogs are populated first.
* Use of a **Foreach Loop Container** to automatically iterate over multiple transactional sales files in a specific directory path and execute the `Carga Hechos_Ventas` task.

### 2. Data Flow (Transformations)
Within each Data Flow Task, raw data undergoes a rigorous transformation process:
* **Derived Column:** String cleansing, standardization, and intermediate calculations.
* **Data Conversion:** Strict adjustment of Data Types from flat files to native SQL Server formats (`INT`, `DATETIME`, `DECIMAL`) to optimize storage and performance.
* **Lookup:** During the fact table load, *Lookup* transformations (Lookup Producto and Lookup Cliente) are used to cross-reference incoming transactional data with the already loaded dimensions, retrieving the correct surrogate `IDs` before the final insertion via the OLE DB Destination.

## Development Evidence
<img width="550" height="799" alt="Captura de pantalla 2026-02-21 012645" src="https://github.com/user-attachments/assets/d9f48137-1961-4e3b-8dd0-971f18688921" />
<img width="1064" height="452" alt="Captura de pantalla 2026-02-21 013346" src="https://github.com/user-attachments/assets/efceecef-6bbb-4d65-a613-54a6ff06d808" />
<img width="608" height="497" alt="Captura de pantalla 2026-02-21 013358" src="https://github.com/user-attachments/assets/b6fcb80e-7b0a-45da-a333-aaa4097943da" />
<img width="721" height="441" alt="Captura de pantalla 2026-02-21 013403" src="https://github.com/user-attachments/assets/36c87fe7-09db-43ad-9163-8224913cbf10" />
<img width="1015" height="522" alt="Captura de pantalla 2026-02-21 021646" src="https://github.com/user-attachments/assets/2ef660bc-bc37-4af0-a69f-1e6818e74524" />
<img width="638" height="798" alt="Captura de pantalla 2026-02-21 013517" src="https://github.com/user-attachments/assets/5da73611-7f28-4a98-b13e-05d2af8d5846" />
## Power bi
<img width="1261" height="711" alt="image" src="https://github.com/user-attachments/assets/0620c820-40fb-43c6-a37d-a61be005af34" />






