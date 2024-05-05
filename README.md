# Car Sales Dashboard
## Overview
This project showcases a car sales dashboard created from a fictional dataset provided during the SQL for Data Analysis course taught by Midori Toyota. The goal was to demonstrate SQL skills and business strategies acquired through my experience in the marketing of aesthetic and wellness equipment.

To maintain confidentiality, the same acquired knowledge was applied to construct the dashboard using fictional data as an alternative.

## Database Structure
The database model comprises four interconnected tables, providing a comprehensive view of business operations. The tables include:

- **Stores:** Details about the network's stores.
- **Customers:** Information about customers in the database.
- **Products:** Description of marketed products.
- **Funnel:** History of customer interactions with the website, offering insights into user behavior.

## Technologies Used
- **SQL:** Utilized for data queries and manipulation in the PostgreSQL database.
- **Business Intelligence:** Implementation of business concepts to enhance efficiency and commercial strategies.
- **Excel:** Specific tools for creating dashboards, providing an intuitive data visualization.

## Repository Structure
The repository contains two files. One in xlsx format, divided into three sheets. The first sheet contains the Car Sales Dashboard. The second displays the results obtained through the queries explained below. The third sheet documents the queries. I opted to include the dashboard in png format due to potential Excel version compatibility issues related to map display.

## Queries Executed

### Query 01 - General Analysis
To create the first chart, Common Table Expressions (CTEs) were employed to calculate the number of leads, sales, total revenue, conversion rate, and average ticket, grouped by month. The CTEs were joined using the month as the joining key.
``` sql
WITH
	leads AS(
		SELECT
			DATE_TRUNC('month', visit_page_date )::date AS visit_page_month,
			COUNT(*) AS visit_page_count
		FROM sales.funnel
		GROUP BY visit_page_month
		ORDER BY visit_page_month
	),
 
	payments AS (
		SELECT
			DATE_TRUNC('month', f.paid_date)::date AS paid_month,
			count(f.paid_date) AS paid_count,
			SUM(p.price *(1 +f.discount)) AS receita
		FROM sales.funnel AS f
		LEFT JOIN sales.products AS p
		ON f.product_id = p.product_id
		WHERE f.paid_date IS NOT null
		GROUP BY paid_month
		ORDER BY paid_month
	)

SELECT
	leads.visit_page_month AS "mês",
	leads.visit_page_count AS "leads(#)",
	payments.paid_count AS "vendas (#)",
	(payments.receita/1000) AS "receita (k, R$)",
	(payments.paid_count:: float/leads.visit_page_count::float) AS "conversão (%)",
	(payments.receita/payments.paid_count/1000) AS "ticket médio (k, R$)"
FROM leads
LEFT JOIN payments
ON leads.visit_page_month = payments.paid_month
```

### Query 02 - Sales by State
Fixing the country as 'Brazil,' a count of the number of sales per state during August 2021 was conducted. A LEFT JOIN between the sales and customers tables was utilized.

``` sql
SELECT
	'Brazil' AS país,
	c.state AS estado,
	COUNT(f.paid_date) AS "vendas (#)"

FROM sales.funnel AS f
LEFT JOIN sales.customers AS c
	ON f.customer_id = c.customer_id
WHERE paid_date BETWEEN '2021-08-01' AND '2021-08-31'
GROUP BY país, estado
ORDER BY "vendas (#)" DESC
LIMIT 5
```

### Query 03 - Brand Analysis
Product sales were analyzed by counting the number of sales per brand, presenting the top five brands with the highest sales.

```sql
SELECT
	p.brand AS marca,
	COUNT(f.paid_date) AS "vendas (#)"
FROM sales.funnel AS f
LEFT JOIN sales.products AS p
	ON f.product_id = p.product_id
GROUP BY marca
ORDER BY "vendas (#)" DESC
LIMIT 5
```

### Query 04 - Sales by Store
Focused on analyzing sales by store, the number of sales per store was counted, and the top five stores with the highest sales were presented.

```sql
SELECT
	s.store_name AS loja,
	COUNT(f.paid_date) AS "vendas (#)"
FROM sales.funnel AS f
LEFT JOIN sales.stores AS s
	ON f.store_id = s.store_id
GROUP BY loja
ORDER BY "vendas (#)" DESC
LIMIT 5
```

### Query 05 - Website Visits by Day of the Week
This query groups the number of website visits by the day of the week, providing insights into traffic patterns throughout the week.
```sql
SELECT
    EXTRACT('dow' FROM visit_page_date) AS dias_semana,
	CASE
		WHEN EXTRACT('dow' FROM visit_page_date) = 0 THEN 'domingo'
		WHEN EXTRACT('dow'FROM visit_page_date) = 1 THEN 'segunda'
		WHEN EXTRACT('dow' FROM visit_page_date) = 2 THEN 'terça'
		WHEN EXTRACT('dow' FROM visit_page_date) = 3 THEN 'quarta'
		WHEN EXTRACT('dow' FROM visit_page_date) = 4 THEN 'quinta'
		WHEN EXTRACT('dow' FROM visit_page_date) = 5 THEN 'sexta'
		WHEN EXTRACT('dow'FROM visit_page_date) = 6 THEN 'sábado'
	ELSE null END AS dias_semana_result,
  COUNT(*) AS num_visitas
FROM sales.funnel
GROUP BY dias_semana
ORDER BY num_visitas
```

## Contributions
Contributions are welcome! Suggestions, corrections, or improvements can be shared through issues or pull requests.

