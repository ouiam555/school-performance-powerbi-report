# Avito Maroc — Power BI Dashboards (Part 2)

Interactive analysis of the Moroccan real estate market using data from Avito.ma

---

## Context

This project is the **second part** of the Avito Maroc data pipeline.

After collecting and cleaning the data (Part 1 — Scraping & Data Cleaning), the data is loaded into a **Data Warehouse** organized into two schemas:

| Schema | Purpose |
|--------|---------|
| `bi_schema` | Star schema — optimized for business intelligence and Power BI analysis |
| `ml_schema` | One Big Table (OBT) — reserved for Machine Learning |

This part focuses **exclusively on `bi_schema`** to build interactive Power BI dashboards.

---

## Data Warehouse Architecture (bi_schema)

```
bi_schema
│
├── fact_avito         <- Fact table (real estate listings)
│   ├── avito_id
│   ├── titre_id
│   ├── ville_id
│   ├── prix
│   ├── prix_m2
│   ├── surface
│   ├── chambres
│   └── salle_de_bain
│
├── dim_regions        <- Geographic dimension
│   ├── ville_id
│   └── ville
│
└── dim_infos          <- Listing dimension
    ├── titre_id
    ├── titre
    └── lien
```

---

## Dashboards

### Dashboard 1 — Market Overview

**Objective:** Provide a high-level view of the real estate market.

**KPIs:**
- Total listings: 340K
- Average market price: 2.05M MAD
- Number of cities covered: 205

**Visualizations:**
- Horizontal bar chart: Top listings by title/residence
- Dynamic filters: city, title, price range

---

### Dashboard 2 — Price Analysis

**Objective:** Analyze price distribution and market segmentation.

**KPIs:**
- Average Price: 2.05M MAD
- Average Price per m2: 24.38K MAD
- Average Surface Area: 99.66 m2
- Maximum Price: 662M MAD
- Minimum Price: 180K MAD

**Visualizations:**
- Average price by city (bar chart)
- Average price per m2 by city
- Dynamic filters: price range, city, price per m2

---

### Dashboard 3 — Geographic Analysis

**Objective:** Visualize the spatial distribution of listings and prices across Morocco.

**KPIs:**
- Total listings
- Average market price
- Number of cities covered

**Visualizations:**
- Colored treemap: listing distribution by city
- Bar chart: Top titles/residences by volume
- Dynamic filters: city, title, price range

---

### Dashboard 4 — Trend Analysis

**Objective:** Analyze the physical characteristics of properties and market trends.

**KPIs:**
- Total Listings: 2K (filtered view)
- Average Surface Area: 74.00 m2
- Average Number of Bedrooms: 2.33
- Average Number of Bathrooms: 1.33

**Visualizations:**
- Average surface area by title
- Average number of bedrooms by city
- Total bathrooms by city
- Dynamic filters: city, bedrooms, surface area

---

## Interactive Filters

All dashboards include dynamic filters:

| Filter | Type |
|--------|------|
| City | Dropdown slicer |
| Title / Property type | Dropdown slicer |
| Price range | Numeric range slider |
| Surface area | Numeric range slider |
| Number of bedrooms | Numeric range slider |

All charts are reactive to selected filters through the relationships defined in the Power BI data model.

---

## DAX Measures

```dax
-- Average price
Average Price = AVERAGE(fact_avito[prix])

-- Total listings
Total Listings = COUNT(fact_avito[avito_id])

-- Average price per m2
Average Price m2 = AVERAGE(fact_avito[prix_m2])

-- Maximum price
Max Price = MAX(fact_avito[prix])

-- Minimum price
Min Price = MIN(fact_avito[prix])

-- Average surface area
Average Surface = AVERAGE(fact_avito[surface])

-- Average number of bedrooms
Average Bedrooms = AVERAGE(fact_avito[chambres])

-- Average number of bathrooms
Average Bathrooms = AVERAGE(fact_avito[salle_de_bain])

-- Number of distinct cities
City Count = DISTINCTCOUNT(fact_avito[ville_id])
```

---

## Power Query Transformations

Before modeling, data was prepared using Power Query:

- Data type verification (price as integer, surface area as integer)
- Removal of remaining outliers
- Validation of relationships between tables
- Creation of calculated columns (`prix_m2 = prix / surface`)
- Filtering of listings missing price or surface area values

---

## Technologies

| Tool | Purpose |
|------|---------|
| Power BI Desktop | Dashboard design and publishing |
| Power Query | Data transformation and preparation |
| DAX | KPI and measure calculation |
| PostgreSQL | Data source (Data Warehouse) |

---

## Getting Started

### Prerequisites

- Power BI Desktop installed
- Access to the PostgreSQL database containing `bi_schema`
- The project `.pbix` file

### Connecting to the Database

1. Open Power BI Desktop
2. Go to **Get Data** -> **PostgreSQL database**
3. Enter the server address and database name
4. Select only the tables from the `bi_schema` schema:
   - `bi_schema.fact_avito`
   - `bi_schema.dim_regions`
   - `bi_schema.dim_infos`
5. Verify the relationships in the **Model** view

---

## Project Structure

```
avito-powerbi/
│
├── dashboards/
│   ├── market_overview.png
│   ├── price_analysis.png
│   ├── geographic_analysis.png
│   └── trend_analysis.png
│
├── avito_maroc.pbix        <- Main Power BI file
└── README.md
```

---

## Future Improvements

- Add an interactive geographic map (Bing Maps / Azure Maps)
- Include time-series analysis of price evolution
- Integrate price predictions from the ML schema
- Publish to Power BI Service for online sharing
- Set up automatic data refresh via Gateway

---

## Author

Created by Ouiam Elkhalfi

Part 1 — Scraping & Data Cleaning: see Part 1 README
