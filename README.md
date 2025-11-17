# 🛡️ Assignment 1: CVE Lakehouse on Databricks

This repository contains the complete implementation of the CVE Data Lakehouse project, following the **Medallion Architecture** pattern (Bronze, Silver, Gold) on **Databricks Community Edition**.

The pipeline ingests raw, semi-structured JSON data, refines it into normalized tables, and generates actionable security intelligence.

***

## ⚙️ Project Overview: Medallion Architecture

[cite_start]The project is structured into three layers of increasing data quality and refinement[cite: 1, 2]:

| Layer | Input | Output | Purpose |
| :--- | :--- | :--- | :--- |
| **Bronze (Raw)** | [cite_start]Raw CVE v5 JSON files from GitHub[cite: 3]. | `cve_bronze.records` Delta Table. | [cite_start]Immutable Single Source of Truth[cite: 3]. |
| **Silver (Cleaned)** | Bronze data. | `cve_silver.core` and `cve_silver.affected` Delta Tables. | [cite_start]Normalized and validated data, cleaned of JSON complexity[cite: 4]. |
| **Gold (Insights)** | Silver tables. | Analytics, charts, and key security insights. | [cite_start]Optimized for consumption and reporting[cite: 5]. |

***

## 🚀 Setup and Execution Instructions

The entire data pipeline is implemented across three sequential Databricks notebooks.

### **Prerequisites**

* [cite_start]**Platform:** Databricks Community Edition[cite: 6].
* [cite_start]**Unity Catalog Volume:** A Unity Catalog Volume must be created (e.g., `/Volumes/workspace/default/cve_demo` [cite: 7]). This path is used to store the persistent Delta files.
* **Required Libraries:** The code requires standard PySpark functions and basic Python libraries (`os`, `json`, `zipfile`, `urllib.request`).

### **Pipeline Execution Order**

[cite_start]Due to the ephemeral nature of the Free Edition cluster, it is safest to run all three notebooks sequentially in one session to ensure table registration persists[cite: 8].

| Notebook | Objective | Notes |
| :--- | :--- | :--- |
| **`01_Bronze_Layer.ipynb`** | Download, filter, and register the raw 2024 CVE data. | [cite_start]This script handles the initial download and Bronze quality checks[cite: 8]. |
| **`02_Silver_Layer.ipynb`** | Normalize and transform Bronze data into two clean tables. | This script incorporates the necessary `from_json` and `explode` fixes for nested data. |
| **`03_Gold_Analysis.sql`** | Run SQL queries on the Silver tables to generate final insights. | This is where the final reporting and charting would occur. |

***

## ✅ Deliverables and Verification Proofs

[cite_start]All deliverables required by the `Assignment1.pdf` rubric [cite: 9] are confirmed below. Corresponding screenshots of the output for verification are located in the `/screenshots` directory of this repository.

### **2. Bronze Layer Implementation (25 Points)**

| Requirement | Verification Status (Code Implementation) | Screenshot Proof |
| :--- | :--- | :--- |
| **Strict Date Filtering** | [cite_start]Filtered by **`cveMetadata.datePublished`** (using `get_json_object`) to enforce 2024-only records, not just folder name[cite: 10]. | `bronze_01_count.png` |
| **Table Registration** | [cite_start]Table successfully registered as **`cve_bronze.records`**[cite: 11]. | `bronze_03_describe_detail.png` |
| **Data Quality Check** | [cite_start]Assertions confirmed: **Volume** ($\ge 30,000$ records), **No Null IDs`, and **Uniqueness**[cite: 12]. | `bronze_02_quality_checks.png` |

---

### **3. Silver Layer Normalization (25 Points)**

| Requirement | Verification Status (Code Implementation) | Screenshot Proof |
| :--- | :--- | :--- |
| **JSON Parsing** | [cite_start]Used the **`from_json`** function to convert both `cveMetadata` and `containers` string columns into usable struct/array types, preventing runtime errors[cite: 13]. | Output of `cve_silver.core` |
| **Explode Operation** | [cite_start]Successfully used **`explode_outer`** to flatten the nested affected products array into the `cve_silver.affected` table[cite: 14]. | Output of `cve_silver.affected` |
| **Core Table** | [cite_start]**`cve_silver.core`** created, including coalesced CVSS scores (prioritizing v3.1)[cite: 15]. | `silver_01_describe_core.png` |
| **Affected Table** | [cite_start]**`cve_silver.affected`** created, demonstrating a successful 1:N relationship model[cite: 16]. | `silver_02_describe_affected.png` |

---

### **4. Gold Analysis & Insights (25 Points)**

| Analysis Component | Key Insight | Screenshot Proof |
| :--- | :--- | :--- |
| **Temporal Analysis** | Calculated the average **disclosure lag** (time between reservation and publication) for 2024 CVEs. | `gold_01_temporal_lag.png` |
| **Risk Distribution** | Identified the count and percentage of vulnerabilities classified as **CRITICAL** or **HIGH** severity. | `gold_02_risk_distribution.png` |
| **Vendor Intelligence** | Ranked the **Top 10 Affected Vendors** by total CVE count found in the normalized data. | `gold_03_top_vendors.png` |

***

## 🏆 Resume-Worthy Accomplishments

* [cite_start]**Medallion Architecture Implementation:** Built a complete **Bronze-Silver-Gold** data pipeline using **Delta Lake** on Databricks[cite: 17].
* [cite_start]**Large-Scale JSON Processing:** Successfully ingested and normalized over **40,000 semi-structured CVE records** by solving complex JSON parsing and array explosion challenges[cite: 18].
* [cite_start]**Data Quality & Defensive Engineering:** Implemented programmatic data quality checks (volume, nulls, uniqueness) at the Bronze layer to ensure data integrity for downstream analytics[cite: 19].
* [cite_start]**Cybersecurity Intelligence:** Developed and executed SQL/PySpark analytics to derive key insights on vulnerability risk distribution and vendor disclosure patterns[cite: 20].
