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

### Query 02 - Sales by State
Fixing the country as 'Brazil,' a count of the number of sales per state during August 2021 was conducted. A LEFT JOIN between the sales and customers tables was utilized.

### Query 03 - Brand Analysis
Product sales were analyzed by counting the number of sales per brand, presenting the top five brands with the highest sales.

### Query 04 - Sales by Store
Focused on analyzing sales by store, the number of sales per store was counted, and the top five stores with the highest sales were presented.

### Query 05 - Website Visits by Day of the Week
This query groups the number of website visits by the day of the week, providing insights into traffic patterns throughout the week.

## Contributions
Contributions are welcome! Suggestions, corrections, or improvements can be shared through issues or pull requests.

