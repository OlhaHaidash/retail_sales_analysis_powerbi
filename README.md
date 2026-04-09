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

Explore dataset: [Fashion Retail Sales Dataset](https://github.com/OlhaHaidash/retail_sales_analysis_powerbi/blob/main/Data%20sources.rar)


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

  ## DAX

I used DAX to build core business logic, calculate key metrics, and enable dynamic analysis within the dashboard.

Developed a dynamic date table and implemented a set of measures to support multi-currency reporting, allowing users to switch between UAH and USD using exchange rates valid for each transaction date.

Created key business metrics, including gross profit, net profit, average order value, profit margin (%), and inventory levels.

Additionally, implemented time intelligence calculations to support period-over-period analysis and predefined date selections.

<details>
  <summary>Inventory Balance Calculation</summary>

  ```

Залишок = 
CALCULATE(
    SUM('рЗакупки'[Кількість]),
    FILTER(
        ALL('Календар'),
        'Календар'[Дата] <= MAX('Календар'[Дата])
    )
)
-
CALCULATE(
    SUM('рПродажіФакт'[Кількість]),
    FILTER(
        ALL('Календар'),
        'Календар'[Дата] <= MAX('Календар'[Дата])
    )
)
```
</details>


<details>
  <summary>Predefined Time Periods (Time Intelligence)</summary>

  ```
Продажі за період = 
SWITCH(
    SELECTEDVALUE('Періоди'[Період]),

    "сьогодні", 
        CALCULATE(
            [Мультивалютні продажі],
            FILTER('Календар', 'Календар'[Дата] = TODAY())
        ),

    "вчора", 
        CALCULATE(
            [Мультивалютні продажі],
            FILTER('Календар', 'Календар'[Дата] = TODAY() - 1)
        ),

    "1е півріччя 2023 року", 
        CALCULATE(
            [Мультивалютні продажі],
            FILTER(
                'Календар',
                'Календар'[Рік] = 2023 &&
                'Календар'[Дата] <= DATE(2023,6,30)
            )
        ),

    "2е півріччя 2023 року", 
        CALCULATE(
            [Мультивалютні продажі],
            FILTER(
                'Календар',
                'Календар'[Рік] = 2023 &&
                MONTH('Календар'[Дата]) >= 7
            )
        ),

    "останні 365 днів", 
        CALCULATE(
            [Мультивалютні продажі],
            FILTER(
                'Календар',
                'Календар'[Дата] >= TODAY()-365 &&
                'Календар'[Дата] <= TODAY()
            )
        ),

    "весь період", 
        [Мультивалютні продажі]
)

```

</details>

## Interactive Dashboard Features

### Landing Page

Designed an interactive landing page that introduces users to the dashboard structure and purpose. It includes a navigation guide with key instructions on how to use filters, interact with visuals, and navigate between reports.

### Revenue & Expenses Analysis

Provides a comprehensive overview of financial performance, including:

- revenue trends over time
- expense structure
- contribution of individual stores to net profit
- revenue breakdown by store
- geographic distribution of sales by product category (map visualization)

### Sales Analysis

Focuses on sales performance and customer behavior:

- sales dynamics and average order value
- product category breakdown
- top 7 best-selling products
- detailed sales table

Enhanced interactivity includes:

- tooltip drill-down: hovering over monthly sales reveals store-level breakdown
- drill-through functionality: access detailed product-level insights from visuals or tables

### Product-Level Analysis (Drill-through)

Dedicated product view with:

- key KPIs for selected product
- inventory levels across stores
- sales trends over time
- sales distribution by store
- detailed table of recent transactions across the network

### Global Filters & Controls

All reports include interactive filters for:

- date (year or specific period)
- currency (multi-currency reporting)
- city and store

A reset button allows users to quickly clear all applied filters.

## Analytical Report & Business Recommendations

### 1. General Business Overview

Profitability: Total Revenue is 27.2M UAH, with a Net Profit of 2.7M UAH. The net profit margin is approximately 10%, which is stable but shows room for expense optimization.

Margin: A high Gross Margin of 47.87% indicates a strong pricing strategy or favorable procurement terms.

Store Efficiency: Most locations are profitable; however, Store №3 shows negative contributions to Net Profit (as seen in the "Store Contribution to Net Profit" waterfall chart).

### 2. Key Insights

Expense Structure: The highest operational costs are Salaries (4.8M) and Rent (3.2M). Marketing spends are relatively low, which might explain the consistent gap in reaching sales targets.

Plan vs. Actual: There is a consistent underperformance compared to the sales plan throughout 2023-2025. The targets might be overly ambitious or not backed by enough marketing activity.

Star Product: "Boots Type 3, Reebok" is the top revenue generator (376k), but the remaining stock is only 31 units. At the current sales rate, the company is facing a high risk of out-of-stock scenarios. Sales are concentrated in Kyiv and Dnipro.

### 3. Strategic Business Decisions

Operational Audit: Conduct a deep dive into Store №3. If operational costs (rent/staff) consistently outweigh marginal profit, consider relocation or closure.

Inventory Management: Urgently restock top-performing items (Reebok, Nike brands). High-velocity items should have a safety stock level higher than current figures.

Target Alignment: Adjust future sales plans to be more realistic (approx. 10-15% lower) or increase the marketing budget to bridge the gap between "Actual" and "Plan."

Average Check Boost: The average transaction value is 3,348 UAH. Implement cross-selling strategies (accessories, socks, cleaning kits) to increase this metric by 5-7%.

