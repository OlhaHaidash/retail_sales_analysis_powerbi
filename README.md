# Power BI Fashion Retail Sales Analysis Dashboard

Power BI dashboard for analyzing revenue and expenses, tracking sales trends, and understanding customer behavior, enabling data-driven insights and answering business-focused questions.

<p align="center">
  <a href="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Income%20and%20Expences.jpg"><img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Income%20and%20Expences.jpg" width="400"/></a>
  <a href="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Sales%20Analysis.jpg"><img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Sales%20Analysis.jpg" width="400"/></a>
  <a href="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Sales%20Plan-Fact.jpg"><img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Sales%20Plan-Fact.jpg" width="400"/></a>
    <a href="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Good's%20card.jpg"><img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Good's%20card.jpg" width="400"/></a>
</p>

## Data Source Description
This dataset includes transactional data from a fashion retail chain covering the period from January 1, 2023 to September 30, 2025. It captures sales across multiple product categories, including footwear, outerwear, accessories, underwear, and headwear.

Data was collected from multiple heterogeneous sources, including structured Excel files, semi-structured data from folder imports, and an external API (National Bank) for historical exchange rates.

Performed data transformation and normalization in Power BI using Power Query, including handling inconsistent schemas, standardizing formats, and merging datasets into a unified data model. This enabled scalable analysis and ensured data consistency across all reporting layers.

Explore dataset: [Fashion Retail Sales Dataset](https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/%D0%94%D0%B6%D0%B5%D1%80%D0%B5%D0%BB%D0%B0%20%D0%B4%D0%B0%D0%BD%D0%B8%D1%85.rar)


## Data Modeling Process

Connected the primary dataset “Budget & Sales (Stores 1–9)”, which includes dimension tables (stores, products, customers) and transactional tables covering actual sales, purchases, operational expenses, and store visitors. Performed initial data preparation by removing unnecessary columns, setting appropriate data types, etc.

Integrated additional data from a folder source “Kharkiv Sales”, where stores use a different order management system and data structure. Standardized and normalized the dataset by aligning store naming conventions using value replacement techniques. Appended Kharkiv transaction data to the main transaction table using Append Queries, and disabled load for intermediate queries to optimize performance.

Imported planning datasets (planned sales and cost of goods sold), which had a hierarchical structure, and transformed them into a flat, analysis-ready format.

Performed data cleaning and normalization across dimension tables:

- Stores: removed empty records and created a standardized store name column for consistent joins
- Customers: extracted unique customer IDs from text fields (e.g., converting “Name 12345” → “12345”) and calculated customer age to support customer segmentation analysis

Built a dynamic date table using DAX (CALENDAR), where the start date is derived from the minimum transaction date and the end date from the maximum planned sales date. This approach ensures the calendar automatically updates as new data is loaded. Additional columns were created, including year, month, day, and day of the week, enabling time-based analysis and identification of monthly and weekly seasonality patterns. 

  <img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Calendar.jpg" width="400"/>

Enriched the dataset by integrating daily exchange rates via an external API ([National Bank](https://bank.gov.ua/NBU_Exchange/exchange_site?start=20230101&end=20301231&valcode=usd&sort=exchangedate&order=desc&json)), enabling dynamic currency conversion and up-to-date financial analysis.

  <img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Currency.jpg" width="1000"/>

Structured the data model by separating fact (transactional) and dimension tables, and established relationships between them to support efficient navigation and multi-dimensional analysis. To improve usability and avoid duplication, key columns used for relationships were hidden in fact tables.

  <img src="https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Images/Table's%20Relationships.jpg" width="1000"/>


