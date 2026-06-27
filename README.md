# School Performance — Power BI Dashboard

Power BI report analyzing student academic performance across courses and teachers.

## Overview

This dashboard was built as part of a data analysis project at Simplon CCFBS. It covers academic performance metrics across students, teachers, and courses, with data cleaned and modeled directly in Power Query.

## Tech Stack

**Power BI Desktop** · **Power Query (M language)** · **DAX**

## Project Structure

```
├── school_performance.pbix   # Power BI report file
└── README.md
```

## Data Model

The report is built on a star schema with:
- Fact table: student grades / evaluations
- Dimensions: students, teachers, courses, time

## Key Dashboards

- Overall academic performance overview
- Per-course grade distribution
- Teacher performance comparison
- Trend analysis over time

## How to Open

Open `school_performance.pbix` in **Power BI Desktop** (Windows only).
