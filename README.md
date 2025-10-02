
                                                                                                                         # World Layoffs Data Cleaning Project

This repository contains the raw and cleaned dataset for **World Layoffs**, along with SQL scripts used to clean, standardize, and prepare the data for analysis. The project demonstrates best practices in data cleaning, including handling duplicates, null values, and inconsistent formats.

This project aims to clean and prepare a global layoffs dataset for further analysis. The cleaning process includes removing duplicates, standardizing text and numeric fields, handling missing or null values, and removing irrelevant rows or columns. By maintaining a structured pipeline, the project ensures reproducibility, safety, and traceability of all transformations.

## Dataset

The repository contains two main files:

- **Raw Data (`layoffs.csv`)**: The original dataset containing layoffs from various companies worldwide.
- **Cleaned Data / SQL Script (`cleanedlayoff.sql`)**: MySQL script containing all the data cleaning steps, including creating staging tables, removing duplicates, standardizing columns, handling nulls, and finalizing the cleaned dataset.

## Database

- **Database name**: `world_layoffs`
- **Final cleaned table**: `layoffs_staging2`
- All operations are performed in MySQL.

## Data Cleaning Steps

First, a staging table (`layoffs_staging`) is created as a copy of the raw table to safely manipulate data without altering the original. Next, duplicates are removed using `ROW_NUMBER()` to assign unique row numbers within groups of identical records, keeping one row per group. Since MySQL cannot delete directly from a CTE, a new table (`layoffs_staging2`) is created to store deduplicated data, and all rows where `row_num > 1` are deleted.

After deduplication, data is standardized: extra whitespace in text columns is trimmed, industry names are unified (e.g., all variations of “Crypto” standardized to `Crypto`), country names are cleaned (removing trailing periods), and the `date` column is converted from text to MySQL `DATE` type using `STR_TO_DATE()` followed by modifying the column datatype.

Null and blank values are handled carefully: missing `industry` values are populated using related rows from the same company, rows where both `total_laid_off` and `percentage_laid_off` are NULL are removed, and numeric columns are assigned `0` where appropriate. Finally, irrelevant columns like `row_num` are dropped after deduplication is complete.

## How to Use

1. Import the raw CSV into MySQL:
```sql
LOAD DATA INFILE 'data/raw/layoffs.csv'
INTO TABLE layoffs
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
````

2. Run the cleaning SQL script:

```sql
SOURCE data/cleaned/cleanedlayoff.sql;
```

3. Access the cleaned data from the table `layoffs_staging2` for further analysis or export.

## Folder Structure

```
world-layoffs-data-cleaning/
│
├── data/
│   ├── raw/
│   │   └── layoffs.csv
│   └── cleaned/
│       └── cleanedlayoff.sql
│
├── README.md
└── LICENSE
```

This structure keeps raw data separate from cleaned data and scripts, making it easier to maintain, reproduce, and audit the cleaning process.

## Author

**Akashreddy Biyyam**

* GitHub: [Your GitHub URL]
* Email: [akashreddybiyyam@gmail.com]


