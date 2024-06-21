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
-- Identify the three best-performing industries based on the number of new unicorns created in 2019, 2020, and 2021 combined.
WITH top_industries AS
(
    SELECT i.industry, 
           COUNT(i.*) AS num_unicorns
    FROM industries AS i
    INNER JOIN dates AS d ON i.company_id = d.company_id
    WHERE EXTRACT(year FROM d.date_joined) IN ('2019', '2020', '2021')
    GROUP BY i.industry
    ORDER BY num_unicorns DESC
    LIMIT 3
),

-- Find the number of unicorns within these industries, the year they became unicorns, and their average valuation in billions.
yearly_rankings AS 
(
    SELECT i.industry,
           EXTRACT(year FROM d.date_joined) AS year,
           COUNT(i.*) AS num_unicorns,
           AVG(f.valuation) AS average_valuation
    FROM industries AS i
    INNER JOIN dates AS d ON i.company_id = d.company_id
    INNER JOIN funding AS f ON d.company_id = f.company_id
    GROUP BY i.industry, year
)

-- Return a table containing industry, year, num_unicorns, and average_valuation_billions.
SELECT yr.industry,
       yr.year,
       yr.num_unicorns,
       ROUND(yr.average_valuation / 1000000000, 2) AS average_valuation_billions
FROM yearly_rankings AS yr
WHERE yr.year IN ('2019', '2020', '2021')
  AND yr.industry IN (SELECT ti.industry FROM top_industries AS ti)
ORDER BY yr.year DESC, yr.num_unicorns DESC;

