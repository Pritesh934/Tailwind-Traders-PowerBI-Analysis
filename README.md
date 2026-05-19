# Enterprise Business Intelligence Solution: Tailwind Traders Sales & Profit Analysis
An end-to-end, production-ready Business Intelligence solution engineered for **Tailwind Traders**, an international retail corporation. This system converts fragmented multidimensional transaction datasets into an optimized star-schema data model, interactive multi-page executive report layouts, mobile-responsive viewports, and automated cloud monitoring pipelines deployed within the **Power BI Service** ecosystem.

---

## 🏛️ System Architecture & File Registry

This project implements a complete enterprise data lifecycle, tracing from raw operational flat files up to automated cloud notification architectures.

```text
tailwind-traders-powerbi-analysis/
│
├── 📁 data/                                            # Operational raw data store
│   ├── 📄 Countries.xlsx                               # Dimension: Geographic lookup coordinates
│   ├── 📄 Purchases.xlsx                               # Dimension: Purchase supply & logistics metrics
│   └── 📄 Tailwind-Traders-Sales.xlsx                  # Fact: Granular operational sales registry
│
├── 📁 documentation/                                   # Portfolio deliverables & verification
│   ├── 📁 Printed Reports/                             # Dynamic PDF exports of dashboard views
│   │   ├── 📄 Executive_Summary_Report.pdf             # Comprehensive all-in-one corporate summary
│   │   ├── 📄 Sales Report.pdf                         # Segment-specific sales activity log
│   │   └── 📄 Profit Overview Report.pdf               # Margin & bottom-line financial overview
│   └── 📁 Screenshots/                                 # Visual presentation assets
│       ├── 📷 executive-dashboard-desktop.png          # Desktop cloud dashboard layout
│       └── 📷 executive-dashboard-mobile.png           # Mobile viewport optimization mockup
│
├── 📄 Tailwind_Traders_Executive_Analysis.pbix         # Master Power BI Analytical Application
└── 📄 README.md                                        # Master Project Documentation

```
## 🛠️ Step-by-Step Technical Implementation

[ Raw Excel Data ] ──► [ Power Query ETL ] ──► [ Star-Schema Model ] ──► [ DAX Measures ] ──► [ Cloud BI Deployment ]

### Phase 1: Local Pre-Processing & Data Preparation (Excel)
To ensure immediate database normalization and reporting alignment, raw transactional line items in the Sales sheet were processed using strict corporate margin parameters:

* **Production Cost Engineering:** Modeled standardized product manufacturing costs at a uniform 35% manufacturing cost allocation margin relative to base product pricing:
  $$\text{Cost Per Unit} = \text{Gross Product Price} \times 0.35$$
* **Raw Revenue Stream Isolation:** Calculated baseline corporate gross inflows across all units processed:
  $$\text{Gross Revenue} = \text{Gross Product Price} \times \text{Quantity Purchased}$$
* **Tax Value Extraction:** Isolated tax values to allow post-tax revenue modeling:
  $$\text{Total Tax} = \text{Tax Per Product} \times \text{Quantity Purchased}$$
* **Net Revenue Modeling:** Formulated operational earnings figures post-tax:
  $$\text{Net Revenue} = \text{Gross Revenue} - \text{Total Tax}$$
* **Net Profit Realization:** Computed pure bottom-line contribution margin per order line:
  $$\text{Profit} = \text{Net Revenue} - \left( \text{Cost Per Unit} \times \text{Quantity Purchased} \right)$$

---

### Phase 2: ETL Pipeline & Structural Schema Harmonization (Power Query)
The raw datasets were fed into the Power Query engine to establish structural integrity, remove anomalies, and clean incoming parameters:

1. **Strict Data Typing:** Enforced numeric representations—casting keys like `OrderID` as **Whole Numbers**, and monetary variables like `Gross Product Price` as **Fixed Decimal Numbers** (Currency) to mitigate downstream floating-point aggregation errors.
2. **Operational Filtering & Returns Isolation:** Analyzed the `Purchases` table's return status metrics. Built conditional filtering paths to isolate and ignore transactional rows with a status of `Returned`, preserving only valid inventory acquisitions.
3. **Data Quality Assurance (DQA):** Utilized advanced Power Query diagnostics (*Column Quality*, *Column Distribution*, and *Column Profile*) to confirm $100\%$ structural completeness, validating that crucial primary and foreign keys contained zero nulls or blank entries.
4. **Advanced Python Script Exchange-Rate Pivot:** Implemented a native Python processing transformation in the ETL query window using `pandas` to dynamically structure, clean, and pivot the currency translation exchange metrics.

---

### Phase 3: Star-Schema Data Modeling & Dimensional Design
To maximize cross-filtering responsiveness and calculation speeds, the relational database model was structured into an optimized analytical **Star-Schema**:

* **Relationship Cardinalities:** Mapped dimensions to core fact tables using robust **One-to-Many ($*:1$)** single-direction filters.
* **Bi-Directional Currency Mapping:** Joined the `Sales in USD` table with the core `Sales` fact table using a bi-directional **One-to-One ($1:1$)** relationship to align real-time currency conversions.
* **Time-Intelligence Calendar Table Generation:** Created a programmatic, dynamic `Calendar Table` in DAX to establish unified corporate accounting periods across all operations:
  ```dax
  Calendar Table = ADDCOLUMNS(
      CALENDAR(DATE(2020, 1, 1), DATE(2023, 12, 31)),
      "Year", YEAR([Date]),
      "Month Number", MONTH([Date]),
      "Month", FORMAT([Date], "MMMM"),
      "Quarter", QUARTER([Date]),
      "Weekday", WEEKDAY([Date]),
      "Day", DAY([Date]))
  
* **USD Conversion Calculated Fields:** Developed consolidated financial metrics in the Sales in USD table by linking transaction metrics with regional exchange rates:
  ```dax
  Sales in USD = ADDCOLUMNS(
    Sales,
    "Country Name", RELATED(Countries[Country]),
    "Exchange Rate", RELATED('Exchange Data'[Exchange Rate]),
    "Exchange Currency", RELATED('Exchange Data'[Exchange Currency]),
    "Gross Revenue USD", [Gross Revenue] * RELATED('Exchange Data'[Exchange Rate]),
    "Net Revenue USD", [Net Revenue] * RELATED('Exchange Data'[Exchange Rate]),
    "Total Tax USD", [Total Tax] * RELATED('Exchange Data'[Exchange Rate]),
    "Profit USD", [Profit] * RELATED('Exchange Data'[Exchange Rate]))
  
### Phase 4: Advanced DAX Measures & Analytics Modeling
To maintain lightweight semantic models and accelerate computation times, explicit measures were engineered to track cross-period performance efficiencies:
* **Yearly Profit Margin:** Analyzed aggregate profitability across annual operational periods (calculated at a stellar 62.30%):
  ```dax
  Yearly Profit Margin = 
  DIVIDE(
    SUM('Sales in USD'[Profit USD]),
    SUM('Sales in USD'[Net Revenue USD]),
    0)
  
* **Quarterly Profit Margin:** Leveraged dynamic time-intelligence context to isolate profitability margins over quarterly tracking cycles (recording 62.27%):
  ```dax
  Quarterly Profit Margin = 
  CALCULATE(
    [Yearly Profit Margin],
    DATESQTD('Calendar Table'[Date]))
  
* **Year-to-Date (YTD) Profit Margin:** Built a running fiscal-year progression metric to track cumulative margins against corporate goals (recording 62.27%):
  ```dax
  Year To Date Profit Margin = 
  TOTALYTD(
    [Yearly Profit Margin], 
    'Calendar Table'[Date])

* **Median Sales Isolation:** Established statistical central tendency thresholds for order size parameters to protect dashboard visual metrics against outlier distortion (calculating a baseline $222.50):
  ```dax
  Median Sales = MEDIAN('Sales in USD'[Gross Revenue USD])

## 📊 Executive Performance Insights

Through deep cross-dimensional visual analytics, the BI solution surfaced key operational findings:

### 1. Revenue & Sales Activity Findings
* **National Loyalty Leaders:** The **United Kingdom (UK)** holds the highest corporate brand alignment, leading the enterprise with **$315$** Loyalty Points, followed closely by the **USA** with **$305$** Points.
* **High-Value Order Hotspots:** Geographic distribution analytics revealed that the **United Arab Emirates (UAE)** yields the highest transaction values. The UAE registered a leading **Median Sales Order Value of $ 680.79**—surpassing the next closest nation (**UK at $ 234.00**) by **$190.93\%$**.
* **High-Velocity Product Volumes:** Inventory analytics flagged the top-selling items by transaction count, led by **Floral Wallpaper**, **Porcelain Dinner Set**, and **ProCarpet Cleaner**, with **$6$** units sold each.

### 2. Profitability & Cost Control Findings
* **Corporate Profit Engine:** Visual product metrics confirmed that the **Modular Sofa Set** is Tailwind Traders' most critical product asset, driving a massive **$ 928.36** in Net Revenue.
* **Low-Yield Structural Risks:** Conversely, **Floral Wallpaper**, despite its high volume ($6$ units sold), is a highly inefficient product line. It generated a net revenue of only **$ 9.60**, highlighting a critical area for pricing adjustment or supplier renegotiation.

---

## 📷 Executive Visualizations & Dashboard Gallery

### Desktop Dashboard Layout
The main dashboard brings together core financial metrics, inventory indicators, and regional profiles.

![Desktop Executive Dashboard](documentation/Screenshots/executive-dashboard-desktop.png)

### Mobile Viewport Layout
An optimized vertical interface constructed for mobile touch targets, ensuring instant, readable layouts for corporate leaders on the move.

![Mobile Executive Dashboard](documentation/Screenshots/executive-dashboard-mobile.png)

---

## ☁️ Enterprise Cloud Infrastructure (Power BI Service)

Once local modeling and visualizations were optimized, the system was published to a dedicated **Power BI Service Capstone Project Workspace** to support executive operations.

### 1. Executive KPIs Dashboard Deployment
The interactive multi-page desktop report was deployed to the cloud. Key metrics (**Gross Revenue**, **Net Revenue**, **YTD Profit Margin**, and **Stock**) were pinned to a central **Tailwind Traders Executive Dashboard** for high-level monitoring.

### 2. Automated Systemic Governance & Alerts
To transform the static report into an active operational assistant, automated triggers and subscriptions were configured directly within the cloud database gateway:
* **Real-Time Data Threshold Alerts:** Programmed a live cloud alert on the **Gross Revenue USD** card to instantly notify stakeholders if transaction inflows drop below a **$\$400$** threshold.
* **Corporate Weekly Subscriptions:** Created scheduled distribution pipelines to email executive teams fresh report page previews and access links on Monday mornings:
  * **Sales Weekly Summary:** Scheduled every **Monday at 5:00 AM** to prepare regional sales teams for the weekly kick-off.
  * **Profit Weekly Summary:** Scheduled every **Monday, Wednesday, and Friday at 6:00 AM** to keep executive leadership aligned on financial performance.

---

## 🎓 Credentials & Professional Scope
* **Curriculum Author:** Authorized and delivered by **Microsoft** through the **Coursera** platform.
* **Technical Competency Domains:** Semantic Modeling, Schema Design, Data Engineering (ETL), DAX Programming, Cloud Analytics, Enterprise Administration, Mobile UI Design, Corporate Governance.
