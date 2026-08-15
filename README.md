# Competitor Sales & Market Share Analysis

![Power BI](https://img.shields.io/badge/Power_BI-Desktop-F2C811?logo=powerbi&logoColor=black&style=flat-square)
![DAX](https://img.shields.io/badge/DAX-Calculations-0078D4?logo=microsoft&logoColor=white&style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)
[![View Dashboard](https://img.shields.io/badge/Power_BI-View_Live_Report-F2C811?logo=powerbi&logoColor=black&style=flat-square)](https://app.powerbi.com/view?r=eyJrIjoiNWZhN2E2NTgtMGZlOC00NGU4LWI1ZDEtMTNlOTdkMWVmMDgxIiwidCI6IjRlMmY1NzE2LTk0ZDMtNGViMC1hZjIyLWI4OTljOTFmN2NkMyIsImMiOjEwfQ%3D%3D)

## 📌 Abstract
Manufacturing enterprises operating in competitive global markets require end-to-end visibility not only into internal product revenue, but also into competitor market penetration and product growth rates. This project develops an executive-ready business intelligence solution in **Power BI** for **Sintec**, a fictional global manufacturer.

By integrating multi-country transactional datasets and building a robust **Snowflake Data Model**, the report provides executives with interactive performance benchmarking, market share diagnostics, and AI-driven root cause analysis. The analysis reveals that Sintec commands a **38.22% market share in the USA** and **19.63% globally**, identifying regional market bottlenecks (e.g., Artisans holding >50% share in Germany) and strategic growth opportunities across product categories.

---

## 📂 Data Ingestion & Preprocessing
The analysis consolidated transactional sales spread across 7 country datasets and 4 dimension sources (2017–2021).

### ETL & Transformation Pipeline (Power Query)
* **Data Integration**: Appended multi-country source files from the International Sales folder with domestic US sales into a single consolidated `Sales` fact table.
* **Data Cleansing**:
  * Cast `Zip` fields from numeric to text to preserve leading zeros and formatting.
  * Handled null values in the `Products` table using **Fill Down** on `Category`.
  * Extracted clean numerical price values and isolated currency codes using **Column from Examples**.
  * Transposed and promoted headers for the `Manufacturer` table.
* **Performance Optimization**: Filtered the transactional scope to the primary 3-year analysis window (2019–2021) and disabled table load on redundant staging queries.

---

## 📈 Exploratory Data Analysis & Business Insights
Interactive exploration and DAX measures were constructed to uncover market trends and category performance.

### 1. Market Share & Revenue Benchmarking
![Competitor Sales Overview](link_to_your_overview_dashboard_image_here)
*Figure 1: Executive Overview — Total Revenue vs. Prior Year Target and Competitor Share Breakdown.*

**Insight:** Sintec holds **21.15% global market share**. While leading in the US (38.22%), European markets present significant competition—primarily from **Artisans**, which dominates over **50% of the German market**.

### 2. Time Intelligence & Growth Trajectories (YoY)
![YoY Growth Trends](link_to_your_growth_trend_image_here)
*Figure 2: Year-over-Year (YoY) Growth Trend across Date and Product Hierarchies.*

**Insight:** Sales achieved peak growth in **Q1 2021 with an 18.8% YoY increase**. Matrix visualizations with rule-based conditional formatting spotlight top-performing product segments exceeding 60% growth.

---

## 🧠 Data Model Architecture & DAX Formulas
A **Snowflake Schema** was implemented to support dimensional hierarchies and prevent bidirectional filtering ambiguity.

* **Surrogate Key Generation**: Generated a concatenated calculated column `ZipCountry` (`Zip & "," & Country`) in both `Sales` and `Geography` tables to resolve non-distinct relationship constraints.
* **Dedicated Calendar Table**: Built using DAX `CALENDAR()` covering the full operational range.

![Data Model Schema](link_to_your_data_model_schema_image_here)
*Figure 3: Power BI Snowflake Data Model Schema and Table Relationships.*

### Core DAX Measures

```dax
// Prior Year (PY) Sales Calculation
PY Sales = 
CALCULATE(
    SUM(Sales[Revenue]),
    SAMEPERIODLASTYEAR('Date'[Date])
)

// Year-over-Year Percentage Growth
% Growth = 
DIVIDE(
    SUM(Sales[Revenue]) - [PY Sales],
    [PY Sales],
    0
)

// Sintec Filtered Revenue
Sintec Revenue = 
CALCULATE(
    SUM(Sales[Revenue]),
    Sales[ManufacturerID] = 4
)

// Sintec Market Share vs. All Competitors
Sintec Market Share = 
DIVIDE(
    [Sintec Revenue],
    CALCULATE(SUM(Sales[Revenue]), ALL(Manufacturer)),
    0
)
```

---

## 📊 Dashboard Architecture & AI Capabilities

| Page / Feature | Architecture | Core Functionality |
| :--- | :--- | :--- |
| **Competitor Sales Analysis** | Executive KPI Dashboard | Gauge target tracking, top 5 competitor breakdown, matrix with heat-mapped % growth, and bookmark-driven storytelling. |
| **Advanced Insights** | AI-Powered Diagnostics | **Decomposition Tree** for category hierarchy root-cause analysis and **Key Influencers** visual to detect revenue drivers. |
| **Interactive UX** | Navigation & Drillthrough | Contextual drillthrough by country, horizontal image-based manufacturer slicers, and custom JSON branded theme formatting. |

---

## 🎯 Strategic Business Recommendations
Based on the dashboard findings, Sintec's leadership team should execute the following data-driven initiatives:

### 1. Counter-Competitor Strategy in Germany
* **Targeted Product Lines**: Develop focused promotional campaigns in segments where Artisans currently dominates market share.
* **Channel Incentives**: Partner with regional distributors to increase shelf space and digital placement in central European markets.

### 2. High-Growth Segment Capitalization
* **Resource Reallocation**: Allocate marketing and R&D capital toward product categories flagged in green (>60% YoY growth) within the matrix analysis.
* **Q1 Seasonality Ramp-up**: Align supply chain and inventory levels ahead of Q1 to capture seasonal demand spikes matching historical peaks.

### 3. Continuous Intelligence Monitoring
* **Automated Drillthrough Audits**: Utilize the AI Key Influencers and Decomposition Trees during monthly executive reviews to detect negative revenue drivers early.

---

## 🔗 Live Interactive Dashboard
Experience the interactive report directly in the browser:

```html
<iframe title="Competitor Sales Analysis" width="100%" height="450" src="[https://app.powerbi.com/view?r=eyJrIjoiNWZhN2E2NTgtMGZlOC00NGU4LWI1ZDEtMTNlOTdkMWVmMDgxIiwidCI6IjRlMmY1NzE2LTk0ZDMtNGViMC1hZjIyLWI4OTljOTFmN2NkMyIsImMiOjEwfQ%3D%3D](https://app.powerbi.com/view?r=eyJrIjoiNWZhN2E2NTgtMGZlOC00NGU4LWI1ZDEtMTNlOTdkMWVmMDgxIiwidCI6IjRlMmY1NzE2LTk0ZDMtNGViMC1hZjIyLWI4OTljOTFmN2NkMyIsImMiOjEwfQ%3D%3D)" frameborder="0" allowFullScreen="true"></iframe>
```

---
*© 2026 Ryan Tang.*
