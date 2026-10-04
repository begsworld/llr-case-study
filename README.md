# llr-case-study
Solutions to LLR's case study problem set.

- **Part 1:** dedupe raw company, person and person-to-company data into clean final tables.
- **Part 2:** match Salesforce accounts to the Part 1 companies and build a company crosswalk.

## Where to look

| File | What's in it |
|---|---|
| `llr_part1_solution.ipynb` | Part 1 solution: dedupe logic, data quality summaries, write-up, risks & monitoring |
| `llr_part2_solution.ipynb` | Part 2 solution: matching logic, crosswalk, write-up |
| `llr_part1_exploratory.ipynb` | Working notes for Part 1 (how the dedupe keys were chosen) |
| `llr_part2_exploratory.ipynb` | Working notes for Part 2 (field checks before matching) |
| `llr.duckdb` | DuckDB database with every table the notebooks create |
| `company.csv`, `person.csv`, `persontocompany.csv`, `account.csv` | Raw input files |

The notebooks were saved with their outputs, so you can read the results on GitHub without running anything. Tables in the notebooks show a preview of up to 10 rows. The full tables are in `llr.duckdb`.

## Final tables

| Table | Rows | Description |
|---|---|---|
| `deduped_company` | 999 | One row per company, deduped on domain (from 1,024 raw) |
| `company_map` | 1,024 | Maps every raw company id to its surviving company id |
| `deduped_person` | 1,863 | One row per person (from 1,870 raw) |
| `deduped_ptc` | 1,870 | One person-to-company link per person (from 1,983 raw) |
| `final_people` | 1,863 | People joined to their surviving company, with an `is_current` flag |
| `company_crosswalk` | 1,124 | 1,024 Part 1 companies + 100 Salesforce accounts, linked by `crosswalk_id` with match confidence and flags |

## Running it

Requires Python 3.11+.

```bash
pip install duckdb jupysql pandas numpy jupyter
jupyter notebook
```

Run `llr_part1_solution.ipynb` first. Part 2 reads `deduped_company`, `raw_company` and `company_map` from `llr.duckdb`, which Part 1 creates.

To query the tables directly:

```python
import duckdb
con = duckdb.connect("llr.duckdb", read_only=True)
con.sql("SELECT * FROM final_people").df()
```
