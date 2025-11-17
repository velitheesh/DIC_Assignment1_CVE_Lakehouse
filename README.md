# 🌐 CVE Lakehouse Pipeline using Databricks Community Edition

A complete end-to-end Lakehouse data engineering project that ingests, processes, and analyzes 2024 CVE (Common Vulnerabilities & Exposures) records using Databricks Community Edition, Delta Lake, and SQL-based analytics.

This repository demonstrates the Bronze → Silver → Gold Lakehouse Architecture, optimized for Databricks CE filesystem limitations.

---

## 🏗️ 1. Architecture Overview

### 🟫 Bronze Layer — Raw Ingestion

| Step | Description |
| :--- | :--- |
| **📥 Download** | Extracts the `cvelistV5` GitHub repository |
| **📄 Load JSON** | Loads all CVE JSON files using `recursiveFileLookup` |
| **🔎 Filter** | Keeps only CVEs published in 2024 |
| **✔ Quality Checks** | ≥ 30,000 records, no null CVE IDs, IDs unique |
| **💾 Store** | Bronze Delta table created in `/databricks/driver` |

### 🥈 Silver Layer — Normalization

**1️⃣ core Table**
Contains normalized CVE metadata:
* CVE ID
* Date published
* Date reserved
* Description
* CVSS score
* CVSS severity
* Status

**2️⃣ affected Table**
Explodes nested JSON structures:

| Column | Meaning |
| :--- | :--- |
| `cve_id` | Vulnerability identifier |
| `vendor` | Vendor or organization impacted |
| `product` | Product, software, or component |

*Enables high-quality vendor-level analytics.*

### 🥇 Gold Layer — SQL EDA

Analysis performed on the Silver tables:
* 📊 Top affected vendors
* 🧭 CVSS severity distribution
* ⏳ Disclosure lag calculations
* 📈 Summary statistics (mean, stddev, max, etc.)

---

## 🗂️ 2. Repository Structure

```text
CVE_Lakehouse_Assignment/
│
├── README.md
│
├── Report/
│   └── CVE_Lakehouse_Final_Report.pdf
│
├── Notebooks/
│   ├── 01_bronze_ingestion.ipynb
│   ├── 02_silver_transformations.ipynb
│   └── 03_sql_eda.ipynb
│
├── Screenshots/
│   ├── bronze_01_count.jpeg
│   ├── bronze_02_quality_checks.jpeg
│   ├── bronze_03_describe_detail.jpeg
│   ├── silver_01_describe_core.jpeg
│   ├── silver_02_describe_affected.jpeg
│   ├── gold_01_temporal_lag.jpeg
│   ├── gold_02_risk_distribution.jpeg
│   └── gold_03_top_vendors.jpeg
│
└── SQL_Queries/
    └── eda_queries.sql
```
---

## ⚙️ 3. Environment Details

| Component | Details |
|----------|---------|
| **Platform** | Databricks Community Edition |
| **Language** | Python (PySpark), SQL |
| **Storage** | `/databricks/driver` (required for CE) |
| **Runtime** | Latest CE-supported Databricks Runtime |
| **Delta Lake** | Yes — used for Bronze/Silver tables |

### ⚠️ Important Notes  
- **Databricks CE blocks DBFS root (`dbfs:/`) access**.  
- All ingestion **must** use the **driver filesystem**  
  (`/databricks/driver/...`).  

---

## 🚀 4. How to Run the Pipeline

### 🔹 STEP 1 — Setup  
1. Create a Databricks CE cluster  
2. Import all three notebooks:  
   - `Bronze Notebook`  
   - `Silver Notebook`  
   - `Gold (SQL EDA) Notebook`  
3. Attach all notebooks to the cluster

---

### 🔹 STEP 2 — Run **Bronze Notebook**

This notebook performs:

- **Download ZIP** — Retrieve *cvelistV5* repository  
- **Extract** — Unzip into `/databricks/driver`  
- **Load JSON** — Read all nested CVE JSON files  
- **Filter** — Keep only **2024 CVEs**  
- **Validate** — Schema + data quality checks  
- **Save** — Store as **Bronze Delta table**

---

### 🔹 STEP 3 — Run **Silver Notebook**

Creates two refined Delta tables:

| Table | Description |
|-------|-------------|
| `cve_silver.core` | Normalized core CVE attributes |
| `cve_silver.affected` | Vendor, product, version breakdown |

---

### 🔹 STEP 4 — Run **Gold (SQL EDA) Notebook**

Produces analytics:

- Severity distribution  
- Vendor-based risk rankings  
- Disclosure lag statistics  
- Summary statistics tables  

---

## 📊 5. Key Insights (Summary)

### 🏆 Top Affected Vendors  
| Vendor | Count |
|--------|-------|
| **Microsoft** | 13,161 |
| **n/a** | 6,591 |
| **Linux** | 6,152 |

---

### 🔥 Severity Distribution  

| Severity | Count | % |
|---------|-------|---------|
| **Medium** | 11,795 | ~30% |
| **High** | 7,588 | ~19% |
| **Critical** | 1,788 | ~4.6% |
| **Low** | 1,015 | ~2.6% |

---

### ⏳ Disclosure Lag Analysis  
- **Average lag:** 50.83 days  
- **Maximum lag:** 686 days  

**Observation:**  
Disclosure timelines vary significantly — suggesting inconsistent security reporting practices among vendors.

---

## 📝 6. Conclusion

This project demonstrates a fully functional **Lakehouse data engineering pipeline** built entirely on **Databricks Community Edition**, overcoming CE’s DBFS limitations.

### ✔️ Key Accomplishments

- Ingested & normalized **thousands of nested CVE JSON files**  
- Implemented **Bronze → Silver → Gold** Delta Lake patterns  
- Executed SQL EDA to derive meaningful cybersecurity insights  
- Designed a **reproducible, scalable pipeline architecture**  
- Followed **modern industry standards** for:
  - Data engineering  
  - Delta Lake modeling  
  - Security analytics  

---


