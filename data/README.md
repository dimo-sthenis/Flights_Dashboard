# Data

This Power BI report is provided as a **template (.pbit)** (no data included).

## Dataset options
- Official source (BTS / DOT On-Time Performance): https://www.transtats.bts.gov/ontime/
- Public mirror (for portfolio replication): https://www.kaggle.com/datasets/usdot/flight-delays

## Expected tables/columns (high level)
Minimum fields typically required:
- FL_DATE (date)
- AIRLINE / CARRIER code
- ORIGIN_AIRPORT, DESTINATION_AIRPORT
- DEP_DELAY, ARR_DELAY (minutes)
- CANCELLED (0/1), DIVERTED (0/1)
- CANCELLATION_CODE (A/B/C/D)
