# Supply Chain Optimization Analytics
## Table of Content
Project Overview

- Data Source
- Tools Used
- Data Preparation and Cleaning
- Exploratory Data Analysis
- Data Analysis
- Key Insights
- Dashboard Preview
- Recommendations
## Project Overview
This project analyzes an end-to-end food and beverage supply chain dataset spanning from initial order placement down to final fulfillment. The primary goal is to empower supply chain managers and executives to track operational performance, identify systemic inefficiencies, isolate operational risks, and directly link logistics disruptions to revenue erosion and financial outcomes.

## Data Source
The transactional dataset for this project was provided by the [ZoomCharts 4U Report Challenge](https://zoomchartswebstorage.blob.core.windows.net/contest/files/ZoomCharts%204U%20Report%20Challenge/Datasets%202026/Supply_Chain_4UReports_Challenge_May_2026.zip) (May 2026), simulating real-world food and beverage distribution scenarios.

## Tools Used
### Power BI
- Data ingestion, profiling, and cleaning
- Star schema data architecture configuration 
- ZoomCharts Custom Visuals
- Multi-level drill-downs and cross-filtering canvas mechanics 

## Data Preparation and Cleaning
- Modeled the flat transactional schema into a dedicated Star Schema containing central fact records and structured dimension maps.
- Formatted inconsistent dates across transactional checkpoints: Order Date, Required Date, and Ship Date.
- Handled empty values inside logistical fulfillment fields where un-shipped records could skew cycle metrics.
- Isolated boolean operational status identifiers including OTIF_Flag, StockoutFlag, and QualityIssueFlag into clean numeric values.

## Exploratory Data Analysis
### EDA was performed using advanced visual drill-downs to resolve these core operational priorities:
- How reliable is the overall delivery pipeline regarding on-time, in-full (OTIF) targets and delivery lag? 
- Where are critical inventory stockouts, product expirations, or operational waste occurrences localized? 
- Which specific vendor suppliers, distribution warehouses, or target customer channels are failing to meet performance benchmarks? 
- How exactly do physical operational errors manifest as direct financial margin leakage and lost profit? 

Data Analysis
Power Query / M Code
Calculating delivery fulfillment lead times via Custom Column arithmetic during data ingestion:

Code snippet
Duration.Days([ShipDate] - [OrderDate])
DAX (Data Analysis Expressions)

OTIF % (On-Time In-Full Ratio):

Code snippet
OTIF % = 
DIVIDE(
    CALCULATE(COUNTROWS('Sales'), 'Sales'[OTIF_Flag] = 1),
    COUNTROWS('Sales'),
    0
) * 100

Order Cycle Duration Safeguard:

Code snippet
LeadDays = 
IF(
    ISBLANK('Sales'[ShipDate]), 
    BLANK(), 
    DATEDIFF('Sales'[OrderDate], 'Sales'[ShipDate], DAY)
)
Key Insights

Operational Disruption Profile: Operational bottlenecks are roughly split evenly, with product quality issues representing 50.09% of financial friction and stockout deficiencies making up the remaining 49.91%.


Financial Erosion: Isolated a macro revenue loss of $37.20K caused entirely by underlying delivery failures and fulfillment stock shortages.


Vendor Bottlenecks: Cross-chart filtering shows that specific suppliers significantly underperform against the baseline 95% target, directly dragging down distribution facility timelines.


Inventory Perishability: Mapping historical inventory longevity trends exposes notable spikes in expired food and beverage quantities during peak stock volume periods.

Dashboard Preview
The analytical control system is arranged into three distinct business views:

Overview Dashboard: Provides executives with macro metrics regarding profit, global revenue ($605.61K), and high-level fulfillment reliability tracking.


Suppliers Dashboard: Leverages a Drill-Down Network structure to outline relational constraints binding suppliers to specific distribution points.


Inventory Dashboard: Examines warehouse shelf health, tracking spoilage trends alongside specific category volume deficits.

(Note: Replace these text placeholders with links to your dashboard screenshots from your GitHub repository folder)

![Overview Dashboard Screen](path/to/Overview_Screenshot.png)

![Suppliers Dashboard Screen](path/to/Suppliers_Screenshot.png)

![Inventory Dashboard Screen](path/to/Inventory_Screenshot.png)

Recommendations

Enforce Vendor SLAs: Use the Vendor Performance & Risk Matrix to hold underperforming suppliers accountable whose delivery rates slip past acceptable cycle parameters.


Balance Channel Inventory: Optimize stock distribution logic across sensitive service accounts like HoReCa, which experience localized fulfillment strains.


Refine Waste Mitigation: Adjust opening stock counts across product lines showing highly recurrent expiration patterns to protect margins from warehouse waste.
