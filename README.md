# ✈️ Flight Operations Analytics (Power BI) — Delays & Cancellations Dashboard

Interactive **Power BI** dashboard for exploring **U.S. domestic flight operations** with a data-science mindset: KPI design, segmentation, correlation analysis, and root-cause breakdown for **delays** and **cancellations**.

> Focus: operational performance monitoring (airlines/airports/months), delay distribution, and cancellation reasons.

---

## Dashboard Preview

![Intro](assets/intro.png)
![All Flights Analysis](assets/all_flights_analysis.png)
![Delayed Flights Analysis](assets/delayed_flights_analysis.png)
![Canceled Flights Analysis](assets/canceled_flights_analysis.png)

---

## Key KPIs (from the report)

### Global / Intro (2015)
- **Flights:** ~**5.82M**
- **Airports:** **322**
- **Airlines:** **14**
- **Avg scheduled time:** **141.69 min**
- **Avg departure delay:** **9.37 min**
- **Avg arrival delay:** **4.41 min**
- **Avg total delay:** **13.76 min**
- **Delayed vs Not delayed:** **36.86%** delayed vs **63.14%** not delayed

### Delayed Flights (filtered to delayed)
- **Delayed flights:** **2.14M**
- **Avg total delay:** **62.93 min**
- **Avg departure delay:** **31.28 min**
- **Avg arrival delay:** **31.81 min**
- **Departure–Arrival delay correlation:** **0.95**

### Cancellations
- **Canceled flights:** **89.88K**
- **Cancellation reasons (share):**
  - **Weather:** **54.35%**
  - **Airline/Carrier:** **28.11%**
  - **NAS (airspace/ATC):** **17.52%**
  - **Security:** **0.02%**

---

## Pages & Analytical Questions Answered

### 1) Intro
- What is the overall traffic volume by month?
- Which airlines & origin airports dominate the network?
- What’s the baseline delay level and seasonality?

### 2) All Flights Analysis
- How do **departure** and **arrival** delays relate (Pearson correlation)?
- Which airports contribute most to average delay?
- Delay segmentation via **Delay_Category** buckets (0, ≤5, 6–15, …, >24h)
- Contribution split: **Departure vs Arrival** delay share

### 3) Delayed Flights Analysis
- Where are delays “heaviest” (top airports / top airlines)?
- How does the **delay magnitude** evolve across months?
- Relationship between departure and arrival delays at scale (scatter + correlation)

### 4) Canceled Flights Analysis
- What is the distribution of cancellation reasons?
- Which airports and airlines drive the most cancellations?
- Cancellation seasonality by month

---

## Data Source (official + common mirror)

This report is typically built on the U.S. DOT / BTS **On-Time Performance** data (aka “Reporting Carrier On-Time Performance”, Part 234).

Official references:
- **BTS Airline On-Time Statistics** (query & download portal):  
  https://www.transtats.bts.gov/ontime/
- **Transtats database overview (On-Time Performance)**:  
  https://transtats.bts.gov/DatabaseInfo.asp?QO_VQ=EFD
- **BTS explainer: delay/cancellation cause categories** (Carrier / NAS / Weather / Security):  
  https://www.bts.gov/topics/airlines-and-airports/understanding-reporting-causes-flight-delays-and-cancellations
- **Part 234 / ASQP on-time report field definitions** (includes cancellation codes A–D):  
  https://esubmit.rita.dot.gov/On-Time-Report.aspx

Common public mirror used in DS portfolios:
- Kaggle dataset: “2015 Flight Delays and Cancellations”  
  https://www.kaggle.com/datasets/usdot/flight-delays

## Data Model (recommended structure)

A clean star schema improves performance and DAX clarity:

- **FactFlights**
  - FlightID / FL_DATE
  - ORIGIN_AIRPORT, DESTINATION_AIRPORT
  - AIRLINE (carrier code)
  - DepDelay, ArrDelay, TotalDelay
  - Cancelled, Diverted
  - CancellationCode (A/B/C/D)
- **DimDate** (calendar table)
- **DimAirline**
- **DimAirport** 

---

## Core Metrics (DAX-style definitions)

Below are typical measures used in this dashboard (names may differ in your file):

- **Total Flights**
  - `COUNTROWS(FactFlights)`
- **Delayed Flights**
  - `CALCULATE([Total Flights], FactFlights[DelayedFlag] = 1)`
- **Delay Rate**
  - `DIVIDE([Delayed Flights], [Total Flights])`
- **Average Departure Delay (min)**
  - `AVERAGE(FactFlights[DepDelay])`
- **Average Arrival Delay (min)**
  - `AVERAGE(FactFlights[ArrDelay])`
- **Average Total Delay (min)**
  - `AVERAGE(FactFlights[TotalDelay])`
- **Pearson Correlation (Arrival vs Departure)**
  - Use `CORREL(FactFlights[DepDelay], FactFlights[ArrDelay])` (or an equivalent approach)

Cancellation reason mapping (BTS):
- `A` = Carrier
- `B` = Weather
- `C` = NAS (National Airspace System)
- `D` = Security


