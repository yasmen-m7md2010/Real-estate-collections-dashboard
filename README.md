# Collection & Financial Risk Intelligence Dashboard

**Enterprise Real Estate Financial Surveillance, Installment Lifecycle DAX Engine & Bank Risk Analytics**

An interactive **Power BI** dashboard that tracks installment collections and credit risk across **8 large-scale real estate projects**, showing what has been collected, what is overdue, and whether the risk sits with banks or with cash customers.

> Graduation project — Data Analysis Diploma, Route Academy

---

## The Portfolio in Numbers

| Total Portfolio Units | Total Active Sold | Sold Rate | Invoiced Demand | Total Overdue Balance |
|:---:|:---:|:---:|:---:|:---:|
| **73,943** | **3,088** | **4.18%** | **13.88B EGP** | **791.01M EGP** |

- **Collected:** 13.09B EGP (~94% of the invoiced demand)
- **Overdue split:** Bank **521.9M EGP** (66%) | Cash **269.1M EGP** (34%)

---

## 1. Business Problem

| Problem | Description |
|---|---|
| **Disparate worksheets & inconsistent plans** | The developer manages 8 compounds across 8 separate Excel workbooks with different payment schedules (for example `20%-20%-20%-15%-15%-5%-5%` vs. `20%-15%-15%-20%-15%-10%-5%`), which fragments the data. |
| **Metric distortions from embedded totals** | The source sheets contain 3 footer summary rows (*Summary Total*, *Paid Installments*, *Remaining Installments*) inside the customer tables, which distorts record counts and averages. |
| **Installment lifecycle blind spots** | There was no way to separate aged delinquencies from future dues. The wide layout (Installments 1–7 as columns) prevented time-series aging and lifecycle tracking. |
| **Obscured credit risk (Bank vs. Cash)** | Management could not separate the credit risk of mortgage banks from the risk of direct in-house cash financing, which delayed legal and collection workflows. |

## 2. Project Mission & Objectives

Build a **100% reconciled Power BI decision-support platform** that ingests the raw project schedules, applies an installment lifecycle model in DAX, and isolates the exposure across banks and cash customers.

- **Standardize ingestion:** an automated Power Query pipeline that removes control rows and normalizes column headers
- **3-state lifecycle modeling:** classify every installment as `PAID`, `DUE - NOT PAID` or `NOT DUE`
- **Time-cutoff analytics (Old vs. New):** parameter-driven slicing to compare historic collections with newly matured dues in any date window
- **Banking surveillance:** exposure ranking per bank to guide liquidity recovery

---

## 3. Data Transformation Pipeline (Power Query)

| Step | Action |
|---|---|
| **1. Source normalization** | Connected to all 8 project workbooks, enforced a standard **21-column schema**, and standardized text encodings, contract numbers and financing institution names |
| **2. Control row isolation** | Removed the final 3 control rows from the customer tables, leaving **3,088 authentic customer contracts** |
| **3. Dimensional unpivoting** | Unpivoted *Installment 1 to 7* into vertical rows indexed by Customer ID, Unit Number, Installment Phase and Transaction Amount |
| **4. Business state categorization** | Added conditional columns mapping each installment to `PAID` (collected), `DUE - NOT PAID` (overdue) or `NOT DUE` (future maturity) |

```
8 Project Workbooks (Excel)
        ↓
Source Normalization (21-column schema)
        ↓
Control Row Removal (3 rows per sheet)
        ↓
Unpivot Installments 1–7
        ↓
3-State Classification (PAID / DUE - NOT PAID / NOT DUE)
        ↓
Power BI Data Model + DAX Measures
        ↓
35-Page Interactive Dashboard
```

---

## 4. DAX Measures

| Measure | DAX Expression | Purpose |
|---|---|---|
| **# Sold** | `COUNTROWS(FILTER(Transactions, Transactions[RowType] = "Customer"))` | Counts active customer records, excluding control rows |
| **Sold %** | `DIVIDE([# Sold], [Adopted Total Units], 0)` | Sales absorption against master-plan units |
| **Issued Invoices** | `CALCULATE(COUNT(Installments[ID]), Installments[State] IN {"PAID","DUE - NOT PAID"})` | Installments that are due, excluding future ones |
| **Collected Invoices** | `CALCULATE(COUNT(Installments[ID]), Installments[State] = "PAID")` | Installments fully collected |
| **Collection Rate** | `DIVIDE([Collected Invoices], [Issued Invoices], 0)` | Efficiency of collecting issued invoices |
| **Invoiced Demand** | `[Collected Amount] + [Outstanding Balance]` | Gross receivables matured across installments P1 to P7 |
| **Bank Outstanding** | `CALCULATE(SUM(Cust[Total Due]), Cust[Bank] <> "Cash" && NOT(ISBLANK(Cust[Bank])))` | Exposure under commercial bank mortgage lines |
| **Cash Outstanding** | `CALCULATE(SUM(Cust[Total Due]), Cust[Bank] = "Cash")` | Direct in-house cash collection risk |
| **Collected OLD / NEW** | `OLD: Date < SelectedStart` &#124; `NEW: SelectedStart <= Date <= SelectedEnd` | Legacy vs. fresh collections in a chosen window |

**Validation principle:** for every project and every bank, the model enforces
`Invoiced Demand = Amount Collected + Outstanding Balance`, and every row reconciles to zero variance.

---

## 5. Portfolio Benchmark

The data model was cross-checked against the validated solution key, and the totals match across all 8 projects.

| Project | Total Units | # Sold | Sold % | Unit Price (EGP) | Invoiced Demand (EGP) | Collected (EGP) | Outstanding (EGP) |
|---|---:|---:|---:|---:|---:|---:|---:|
| One Kattameya Compound | 3,572 | 640 | 17.92% | 3,405,310,000 | 2,493,322,150 | 2,370,514,900 | 122,807,250 |
| Zahra North Coast | 25,132 | 20 | 0.08% | 145,374,000 | 145,374,000 | 118,002,700 | 27,371,300 |
| Degla Landmark | 5,082 | 600 | 11.81% | 2,946,844,000 | 2,946,844,000 | 2,791,641,350 | 155,202,650 |
| Skyline Katamya Compound | 13,500 | 528 | 3.91% | 5,667,579,000 | 5,384,200,050 | 5,186,249,300 | 197,950,750 |
| Degla Palms 6 October | 23,928 | 279 | 1.17% | 1,309,473,000 | 597,703,250 | 516,028,550 | 81,674,700 |
| Lake Front 6 | 1,203 | 249 | 20.70% | 1,380,254,000 | 276,050,800 | 207,128,600 | 68,922,200 |
| Crystal Plaza Maadi | 635 | 317 | 49.92% | 1,519,421,000 | 1,017,194,800 | 975,515,950 | 41,678,850 |
| Rihana | 891 | 455 | 51.07% | 2,265,960,000 | 1,018,338,250 | 922,932,625 | 95,405,625 |
| **Portfolio Total** | **73,943** | **3,088** | **4.18%** | **18,640,215,000** | **13,879,027,300** | **13,088,013,975** | **791,013,325** |

---

## 6. Key Insights

### 1. Exposure concentration in Skyline Katamya
Skyline Katamya carries the highest single exposure: **197.95M EGP** unpaid, which is **25%** of the total portfolio debt. **122.92M EGP (62%)** of it is held by direct cash customers, which puts company working capital at direct risk.

### 2. Bank risk allocation
Commercial banks account for **66% (521.88M EGP)** of total outstanding debt across **599 financed clients**. The debt is concentrated in a few bank and project combinations. In One Kattameya, one bank shows a **36.5% default rate** (23.22M EGP overdue out of 63.51M EGP).

### 3. Installment aging decay (P4–P5)
Installments **P1 to P3** consistently reach **above 96%** collection. Collection then drops to **85.77% at P4** and **83.87% at P5**, which identifies the maturity window where follow-up is needed most.

### 4. Fully realized project outliers
**Zahra North Coast** is a 100% direct-cash project (no bank reliance, 20 units sold, 118M EGP collected). **Rihana** (51.07% sold) and **Crystal Plaza Maadi** (49.92% sold) lead sales absorption and show stable collection compliance.

---

## 7. Recommendations

| Recommendation | Detail |
|---|---|
| **Bank workout taskforce** | Set up bi-weekly reconciliation with the credit risk committees of the banks with the largest delinquencies, and restructure them under formal expedited drawdown agreements |
| **Predictive P4–P5 interventions** | Configure automated alerts (Power Automate / CRM) at **Day -60** before Installment 4 maturity, to prevent the drop from 96% to below 85% |
| **Early-settlement incentives** | Offer a **3%–5% cash discount** to Skyline's **373 cash buyers** holding **122.92M EGP** in overdue balance, to bring in liquidity sooner |
| **Source data governance** | Remove manual summary rows at the source workbook level so the Power BI Service gateway can refresh automatically |

---

## 8. Report Architecture & Design

The report has **35 pages** with a dark corporate theme and a consistent layout.

| Pages | Content |
|---|---|
| **Page 1: Home Hub** | Central navigation portal with bookmarks to every view |
| **Pages 2–3: Global Views** | Cross-project Portfolio Summary and a date-sliced (Old vs. New) audit matrix |
| **Pages 4–35: Project Details** | 8 projects × 4 views: **Overall**, **Cash**, **Bank** and **Bank-Wise** |

**Design standards**
- **Persistent global header** with a Home button, Project Units and a dynamic POC% indicator
- **Semantic colors:** emerald for collected, crimson for delinquent
- **Structured matrices** that prevent overflow and export cleanly to PDF
- Designed in **Figma** and imported into Power BI as page backgrounds
- Slicers for project, date, installment number and bank, plus navigation buttons on every page

---

## Tools & Technologies

- **Power BI Desktop:** data modeling, visuals, navigation
- **Power Query:** ETL (normalization, control-row removal, unpivoting, state classification)
- **DAX:** lifecycle and risk measures
- **Figma:** dashboard UI design
- **Excel:** source data (8 project workbooks)

## Files

| File | Description |
|---|---|
| `Graduation Project (2).pbix` | The Power BI report |
| `Graduation Project Data Source.xlsx` | The source data used in the dashboard |
| `Morshedy_8_Projects_Advertised_Total_Units (1).xlsx` | Total advertised units for the 8 projects |

**How to open:** download the `.pbix` file and open it with **Power BI Desktop** (free, Windows only).

## Skills Demonstrated

Power BI · Power Query (ETL) · DAX · Data Modeling · Data Cleaning · Financial & Credit Risk Analysis · KPI Development · Data Visualization · UI Design (Figma) · Business Analysis

---

## Author

**Yasmen Mohamed**
[LinkedIn](https://www.linkedin.com/in/yasmen-mhmd) · [Email](mailto:yasmen.m7md36@gmail.com)
