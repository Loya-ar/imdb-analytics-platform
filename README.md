# IMDb Data Analytics Platform - Business Intelligence Implementation

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
| Data Profiling | Alteryx | Data quality analysis and profiling |
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
- Analyze data distributions using Alteryx/YData
- Document data quality issues
- Create data profiling reports

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
- `fact_title_ratings` - 1.6M rows
- `fact_episodes` - 9.3M rows  
- `fact_title_principals` - 96M rows

### Dimension Tables (6)
- `dim_title` - 12M rows
- `dim_name` - 14.9M rows
- `dim_genre` - 28 unique genres
- `dim_profession` - 46 unique professions
- `dim_region` - 252 unique regions
- `dim_language` - 111 unique languages

### Bridge Tables (3)
- `bridge_title_genre` - 24M rows
- `bridge_name_profession` - 35M rows
- `bridge_title_akas` - 54M rows

## ✅ Data Quality Standards

### Validation Rules (60 total)
- Primary key constraints
- Date range validations (1800-2030)
- NULL handling for required fields
- Format standardization (e.g., tt* prefix for title IDs)
- Cross-field validations
- Domain-specific checks (e.g., valid title types)

### Audit Columns (All Tables)
- `DI_JOB_ID` - Job identifier for traceability
- `DI_LOAD_DT` - Load timestamp
- `_source_file` - Original source file reference

### Quality Metrics
- **Data Quality Score:** 99.9997%
- **Total Validations:** 60
- **Rows Dropped:** 646 (0.0003%)
- **Drop Reasons:** Invalid dates, NULL primary keys, format violations

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

### Dashboard Components
1. **Movie Insights Dashboard**
   - Release trends by year
   - Genre distribution analysis
   - Rating correlations

2. **Crew Analytics Dashboard**
   - Actor performance metrics
   - Director and writer popularity
   - Multi-profession analysis

3. **Content Trends Dashboard**
   - Year-over-year comparisons
   - Movie length analysis
   - Adult vs non-adult content

4. **Regional Analysis Dashboard**
   - Country distribution
   - Language diversity metrics
   - Regional release patterns

5. **Series Insights Dashboard**
   - Season performance analysis
   - Episode count trends
   - Series vs movie comparisons

## 🚀 Getting Started

### Prerequisites
- Databricks workspace access
- Power BI Desktop/Pro
- Alteryx Designer (for data profiling)
- ER Studio (for data modeling)
- Git for version control

### Installation

1. Clone the repository
```bash
git clone https://github.com/yourusername/imdb-bi-project.git
cd imdb-bi-project
```

2. Download IMDb datasets
```bash
# Download from IMDb Non-Commercial Datasets
wget https://datasets.imdbws.com/name.basics.tsv.gz
wget https://datasets.imdbws.com/title.basics.tsv.gz
# ... download all 7 files
```

3. Extract the files
```bash
gunzip *.gz
```

4. Set up Databricks environment
   - Create Delta Live Tables pipeline
   - Configure compute cluster
   - Set up storage locations

5. Run the pipeline
   - Execute Bronze layer notebooks
   - Run Silver layer validations
   - Process Gold layer transformations

6. Connect Power BI to Databricks
   - Configure Databricks connector
   - Import data model
   - Deploy dashboards

## 📂 Project Structure

```
imdb-bi-project/
├── README.md
├── documentation/
│   ├── Final_Mapping_Doc.xlsx
│   ├── Data_Profiling_Report.pdf
│   └── ER_Model.erwin
├── databricks/
│   ├── bronze/
│   │   └── bronze_ingestion.py
│   ├── silver/
│   │   └── silver_validation.py
│   └── gold/
│       └── gold_transformation.py
├── alteryx/
│   └── data_profiling_workflow.yxmd
├── powerbi/
│   └── IMDb_Analytics.pbix
├── sql/
│   ├── create_tables.sql
│   └── business_queries.sql
└── tests/
    └── validation_tests.py
```

## 📋 Deliverables

- [x] Complete mapping document with source-to-target mappings
- [x] Data profiling analysis reports
- [x] ER model and schema design files
- [x] Source code in Git repository
- [x] Validation and audit logs
- [x] Power BI dashboards
- [x] Project presentation (5 slides)

## 👥 Team Contributions

Document team member contributions here:
- Member 1: Data modeling, Bronze layer implementation
- Member 2: Silver layer validations, data quality
- Member 3: Gold layer transformations, star schema
- Member 4: Power BI dashboards, visualizations

## 📚 References

- [IMDb Non-Commercial Datasets](https://developer.imdb.com/non-commercial-datasets/)
- [ISO 639 Language Codes](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
- [Country Codes for Movies](https://www.imdb.com/country/)
- Databricks Medallion Architecture Best Practices
- Power BI Best Practices for Large Datasets

## 📝 License

This project uses IMDb's non-commercial datasets and is for educational purposes only.

## 🤝 Acknowledgments

- DAMG 7370 Course Instructors and TAs
- IMDb for providing non-commercial datasets
- Databricks for Delta Live Tables platform

---

**Project Status:** ✅ Complete  
**Last Updated:** December 2025
