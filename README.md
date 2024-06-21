# AnalyzingUnicornCompanies
# Unicorns Analysis

This repository contains an SQL query to identify and analyze the top-performing industries based on the number of new unicorns created in the years 2019, 2020, and 2021. The analysis focuses on the top three industries with the most unicorns and provides detailed insights into the number of unicorns per year and their average valuation in billions.

## Database Schema

The unicorns database contains the following tables:

### `dates`
| Column        | Description                             |
| ------------- | --------------------------------------- |
| `company_id`  | A unique ID for the company.            |
| `date_joined` | The date that the company became a unicorn. |
| `year_founded`| The year that the company was founded.  |

### `funding`
| Column            | Description                                 |
| ----------------- | ------------------------------------------- |
| `company_id`      | A unique ID for the company.                |
| `valuation`       | Company value in US dollars.                |
| `funding`         | The amount of funding raised in US dollars. |
| `select_investors`| A list of key investors in the company.     |

### `industries`
| Column       | Description                           |
| ------------ | ------------------------------------- |
| `company_id` | A unique ID for the company.          |
| `industry`   | The industry that the company operates in. |

### `companies`
| Column       | Description                             |
| ------------ | --------------------------------------- |
| `company_id` | A unique ID for the company.            |
| `company`    | The name of the company.                |
| `city`       | The city where the company is headquartered. |
| `country`    | The country where the company is headquartered. |
| `continent`  | The continent where the company is headquartered. |

## SQL Query

The SQL query identifies the top three industries with the highest number of new unicorns from 2019 to 2021, then calculates the number of unicorns per year and their average valuation in billions. The results are sorted by year and number of unicorns in descending order.

```sql
-- SQL query provided here
