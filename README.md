# IMDb Data Analytics Platform — Business Intelligence Implementation

End-to-end BI solution over IMDb's non-commercial datasets using medallion
architecture. Processes **200.28M records across 7 source files** into a
dimensional model serving movie and personnel analytics.

**Course:** DAMG 7370 — Designing Data Architecture for Business Intelligence,
Northeastern University

## 📊 Project Overview

This project implements an end-to-end Business Intelligence solution for analyzing IMDb's non-commercial datasets using medallion architecture. The solution processes over 200 million records across 7 source files to create a comprehensive analytical platform for movie and personnel insights.

## 🎯 Business Objectives

The platform enables business analysts to:
- Analyze personnel professions and identify multi-talented individuals
- Discover movies by genre and track release trends
- Monitor movie lengths and ratings across years
- Track regional and language distributions
- Compare TV series performance with movies
- Identify popular directors, writers, and actors

## 🏗️ Architecture

### Medallion Architecture Implementation
```
Bronze Layer (Raw) → Silver Layer (Validated) → Gold Layer (Curated)
```

- **Bronze Layer:** Raw data ingestion from TSV files (200.28M rows)
- **Silver Layer:** Cleaned and validated data with 60 quality checks (200.28M rows, 0.0003% drop rate)
- **Gold Layer:** Dimensional model with star schema (~235M rows across 12 tables)

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| Data Modeling | ER Studio | Database design and schema modeling |
| Data Profiling | Alteryx & DQX | Data quality analysis and profiling |
| ETL Processing | Databricks | Data transformation and pipeline orchestration |
| Storage | Delta Live Tables | Data lake storage with ACID compliance |
| Visualization | Power BI | Interactive dashboards and reporting |
| Version Control | Git | Code collaboration and versioning |

## 📁 Data Sources

| File | Description | Row Count | Key Columns |
|------|-------------|-----------|-------------|
| `name.basics.tsv` | Cast members and personnel details | 14,903,875 | nconst (PK), primaryName, primaryProfession |
| `title.basics.tsv` | Movie/TV title details and genres | 12,095,664 | tconst (PK), titleType, primaryTitle, genres |
| `title.akas.tsv` | Alternate titles in multiple languages | 54,124,127 | titleId+ordering (PK), title, region, language |
| `title.crew.tsv` | Directors and writers for titles | 12,095,664 | tconst (PK), directors, writers |
| `title.episode.tsv` | TV series episodes with season info | 9,324,110 | tconst (PK), parentTconst, seasonNumber |
| `title.principals.tsv` | Principal cast/crew credits | 96,132,477 | tconst+ordering (PK), nconst, category |
| `title.ratings.tsv` | Movie ratings and vote counts | 1,605,649 | tconst (PK), averageRating, numVotes |

**Total Records:** 200,281,566

## 🔄 Data Pipeline

### 1. Data Profiling Phase
- Analyze data distributions using Alteryx, DQX, and YData
- Document data quality issues
- Create comprehensive profiling reports
- Generate data quality metrics

### 2. Bronze Layer Processing
- Raw TSV file ingestion
- Source tracking metadata
- No transformations applied
- 100% data retention

### 3. Silver Layer Processing
- **60 validation rules** applied
- Data type standardization
- NULL value handling
- Invalid date corrections
- **Drop rate:** 646 rows (0.0003%)

### 4. Gold Layer Processing
- Star schema implementation
- Dimension tables creation
- Fact tables aggregation
- Bridge tables for many-to-many relationships

## 📊 Data Model

### Fact Tables (3)
- `fact_title_ratings` — 1.6M rows
- `fact_episodes` — 9.3M rows
- `fact_title_principals` — 96M rows

### Dimension Tables (6)
- `dim_title` — 12M rows
- `dim_name` — 14.9M rows
- `dim_genre` — 28 unique genres
- `dim_profession` — 46 unique professions
- `dim_region` — 252 unique regions
- `dim_language` — 111 unique languages

### Bridge Tables (3)
- `bridge_title_genre` — 24M rows
- `bridge_name_profession` — 35M rows
- `bridge_title_akas` — 54M rows

## ✅ Data Quality Standards

### Validation Rules (60 total)
- Primary key constraints
- Date range validations (1800–2030)
- NULL handling for required fields
- Format standardization (e.g., `tt*` prefix for title IDs)
- Cross-field validations
- Domain-specific checks (e.g., valid title types)

### Audit Columns (All Tables)
- `DI_JOB_ID` — Job identifier for traceability
- `DI_LOAD_DT` — Load timestamp
- `_source_file` — Original source file reference

### Quality Metrics
- **Total validations:** 60 rules across the Silver layer
- **Rows dropped:** 646 of 200,281,566 (0.0003%)
- **Drop reasons:** Invalid dates, NULL primary keys, format violations

## 📈 Business Analytics Capabilities

### 15 Implemented Business Requirements

1. Get different types of professions for any given personnel
2. Generate report for personnel with multiple professions
3. Get list of genres for a given title
4. Identify list of movies based on given genre
5. Find all movies released in a given year
6. Track movie length metrics by release year
7. List all adult and non-adult movies
8. Generate all languages for a title
9. Get list of regions for movie releases
10. Get list of directors and writers for a movie
11. Get episodes per season for a title
12. Find all crew/cast types for a title
13. Get list of jobs for a title
14. Get list of characters for a title
15. Find top-rated movies by year and genre

## 📊 Power BI Dashboards

Built against the Gold layer via the Databricks connector.

1. **Movie Insights** — release trends by year, genre distribution, rating correlations
2. **Crew Analytics** — actor performance, director and writer popularity, multi-profession analysis
3. **Content Trends** — year-over-year comparisons, movie length analysis, adult vs non-adult
4. **Regional Analysis** — country distribution, language diversity, regional release patterns
5. **Series Insights** — season performance, episode count trends, series vs movie comparisons

## 📂 Repository Contents

This repository holds the profiling workflows, pipeline notebooks, mapping
document, and data model. The Databricks Delta Live Tables pipelines and the
Power BI report were developed in their respective workspaces and are not
checked in here.

```
Final-Project--DAMG7370/
├── README.md
├── Main_Pipeline.ipynb            # Bronze → Silver → Gold transformations
├── Data_Profiling_complete.ipynb  # Profiling and data quality analysis
├── Final_Mapping_Doc.xlsx         # Source-to-target mapping specification
├── final_project_team.DM1         # ER Studio data model
└── Alteryx profiling workflows    # one per source file
    ├── name_basic.yxmd
    ├── title_akas.yxmd
    ├── title_basics_.yxmd
    ├── title_crew.yxmd
    ├── title_episode.yxmd
    ├── title_principle.yxmd
    └── title_ratings.yxmd
```

## 🚀 Getting Started

### Prerequisites
- Databricks workspace access
- Power BI Desktop/Pro
- Alteryx Designer (to open the `.yxmd` profiling workflows)
- ER Studio (to open `final_project_team.DM1`)

### Setup

1. Clone the repository
```bash
git clone https://github.com/Loya-ar/Final-Project--DAMG7370.git
cd Final-Project--DAMG7370
```

2. Download the IMDb datasets
```bash
wget https://datasets.imdbws.com/name.basics.tsv.gz
wget https://datasets.imdbws.com/title.basics.tsv.gz
# ... repeat for all 7 files
gunzip *.gz
```

3. Set up the Databricks environment
   - Create the Delta Live Tables pipeline
   - Configure the compute cluster and storage locations
   - Upload the source TSV files to the Bronze location

4. Run `Main_Pipeline.ipynb`
   - Bronze ingestion → Silver validation → Gold transformation

5. Connect Power BI to Databricks and build against the Gold layer

## 📋 Deliverables

- [x] Complete mapping document with source-to-target mappings
- [x] Data profiling analysis reports
- [x] ER model and schema design files
- [x] Source code in Git repository
- [x] Validation and audit logs
- [x] Power BI dashboards
- [x] Project presentation

## 👥 Team

Four-person team project for DAMG 7370.

**Arjun Loya** — ER/dimensional modeling and Bronze ingestion, Silver layer
validation rules and data quality checks, Gold layer star schema
transformations, and Power BI dashboards.

Delivered collaboratively across data modeling, validation, dimensional
modeling, and reporting.

## 📚 References

- [IMDb Non-Commercial Datasets](https://developer.imdb.com/non-commercial-datasets/)
- [ISO 639 Language Codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
- Databricks Medallion Architecture Best Practices
- Power BI Best Practices for Large Datasets

## 📝 License

This project uses IMDb's non-commercial datasets and is for educational purposes only.

## 🤝 Acknowledgments

- DAMG 7370 Course Instructors and TAs
- IMDb for providing non-commercial datasets
- Databricks for the Delta Live Tables platform

---

**Status:** Complete — December 2025
