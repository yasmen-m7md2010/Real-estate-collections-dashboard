# Real Estate Collections Analysis

## Project Overview

This project analyzes **installment collections across 8 real estate projects** using **Power BI**. 

The source data was provided as Excel files, and all the work (cleaning, modeling, calculations and visuals) was done in Power BI.

The dashboard design was created in **Figma**.

The goal is to turn raw installment and payment data into an interactive dashboard that shows how much has been collected, how much is still outstanding, and how cash and bank-financed units perform in each project.

---

## Business Questions

The dashboard is designed to answer the following questions:

1. **How much has been collected vs. how much is still outstanding, per project?**
2. **How far along is each project (completion %) and how many units are sold?**
3. **How do collections differ between cash and bank-financed units?**
4. **Which banks hold the largest outstanding amounts?**
5. **What is the status of each installment, and how does it change by date?**

---

## Tools & Technologies

### Figma
- Designing the dashboard layout, side navigation menu and page backgrounds
- Exporting the designs and using them as page backgrounds in Power BI

### Power BI
- Importing the source data (provided as Excel files)
- Cleaning the data and combining the project tables (Append) using Power Query
- Building the data model
- Creating DAX measures
- Building KPI cards and interactive visuals
- Adding slicers, navigation buttons and a custom design

---

# Project Workflow

## 1. Data Cleaning & Consolidation in Power Query
The data for each project was cleaned in Power Query, and the tables of the 8 projects were then combined using **Append**. The result is a single table (`All_Projects_Raw`) that contains the data of all 8 projects, with fields such as *Project Name*, *Installment No*, *Installment State*, *Payment Method*, *Bank* and *Date*.

## 2. Data Modeling in Power BI
The model contains:

| Table | Purpose |
|---|---|
| `All_Projects_Raw` | Main data table (result of appending the 8 projects) |
| `All DAXs` | Table that holds all DAX measures |
| `Date Selection` | Helper table for the date slicer |
| `Installment Selector` | Helper table for the installment-number slicer |

## 3. DAX Measures

| Measure | Meaning |
|---|---|
| Collected Amount | Total amount already collected |
| Outstanding Amount | Total amount still to be collected |
| Outstanding % | Outstanding amount as a share of the total |
| Collection Rate | Share of the due amount that has been collected |
| Total Units | Total number of units in the project |
| # Sold | Number of sold units |
| Sold % | Sold units as a share of total units |
| Project Completion % | How far the project is in terms of collections and sales |
| Project Sold Rows | Number of sold records in the project |
| Issued / Collected / Outstanding Invoices | Invoice counts by status |
| Invoice Amount | Total invoiced amount |
| Unit Price | Price of the unit |
| Bank Outstanding / Cash Outstanding | Outstanding amount split by payment type |

---

# Dashboard Design

The report has **35 pages** with a dark, gold-accented theme, a side navigation menu, and a consistent layout across all projects. The design was created in Figma and imported into Power BI as page backgrounds.

| Section | Pages |
|---|---|
| General | Home Page, Global Summary, Collection By Date |
| Projects | 8 projects × 4 pages each = 32 pages |

Each project has the same four pages:

| Page | What it shows |
|---|---|
| **Overall** | Installment state and collected vs. outstanding per installment |
| **Cash** | Collections for cash-paying units |
| **Bank** | Collections for bank-financed units and the top 5 banks by outstanding amount |
| **Bank-Wise** | Drill-down by bank, with unit price by payment method |

### Projects covered

1. One Kattameya Compound
2. Zahra North Coast
3. Degla Landmark
4. Skyline Katamya Compound
5. Degla Palms 6 October Compound
6. Lake Front 6
7. Crystal Plaza Maadi Compound
8. Rihana

---

## KPI Section

Each project page starts with four KPI cards:

- **Records in current segment**
- **Project Completion %**
- **Adopted Total Units**
- **Project Sold Rows**

---

# Visualizations

## Installment State (Donut Chart)
Shows the split of installments by status.

## Collected vs. Outstanding by Installment (Bar Chart)
Compares the collected and outstanding amounts for each installment number.

## Top 5 Banks – Outstanding (Bar Chart)
Ranks the banks with the highest outstanding amounts.

## Unit Price by Payment Method (Donut Chart)
Shows how unit value is split between cash and bank payments.

## Slicers & Navigation
- Filter by **Project**, **Date**, **Installment Number** and **Bank**
- Navigation buttons on every page to move between Overall, Cash, Bank and Bank-Wise

---

# End-to-End Pipeline

```
Raw Installment Data (Excel files)
        ↓
Data Cleaning & Preparation
        ↓
Power BI Data Model
        ↓
DAX Measures (KPIs & Calculations)
        ↓
Visualizations, Slicers & Navigation
        ↓
Interactive Collections Dashboard
```

---

# Files

| File | Description |
|---|---|
| `Graduation Project (2).pbix` | The Power BI report |
| `Graduation Project Data Source.xlsx` | The source data used in the dashboard |
| `Morshedy_8_Projects_Advertised_Total_Units (1).xlsx` | Total advertised units for the 8 projects |

## How to Open
1. Download the `.pbix` file.
2. Open it with **Power BI Desktop** (free, Windows only).

---

# Project Outcome

An interactive Power BI dashboard that converts raw installment data into clear, actionable insights on collections, outstanding balances and bank exposure across 8 real estate projects.

# Skills Demonstrated

- Power BI
- DAX
- Data Modeling
- Data Cleaning & Preparation
- KPI Development
- Data Visualization
- UI Design (Figma)
- Dashboard Design & Navigation
- Business Analysis

---

## Author

**Yasmen Mohamed**
[LinkedIn](https://www.linkedin.com/in/yasmen-mhmd) · [Email](mailto:yasmen.m7md36@gmail.com)
