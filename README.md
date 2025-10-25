# IVT Analysis — Ad Fraud Detection
This repository analyzes Invalid Traffic (IVT) patterns across six mobile apps using hourly change‑point detection to explain why some apps were flagged and others remained valid. 

## Overview
The analysis identifies the first hour where each app’s IVT exceeds 0.5 and then quantifies before/after shifts in key metrics such as idfa_ua_ratio and requests_per_idfa. 
Three apps are flagged at discrete hourly change‑points while three apps never cross the threshold during the observation window. 
Post‑spike IVT stabilizes near 1.0 for flagged apps, while idfa_ip_ratio remains ≈1 and requests_per_idfa clusters ≈1.06–1.11, indicating a source or signature shift rather than IP crowding or request‑rate surges. 
Data window: 2025‑09‑11 to 2025‑09‑15 as recorded in the cleaned masters. 

## Data and Inputs
- Raw workbook: data/Data-Analytics-Assignment.xlsx (six sheets: 3 Valid, 3 Invalid).
- Cleaned daily table: outputs/daily_master_clean.csv.
- Cleaned hourly table: outputs/hourly_master_clean.csv.
- Per‑app summary stats: outputs/summary_by_app.csv.
- Change‑points per app: outputs/first_high_IVT_per_app.csv.
- App flag classification: outputs/app_flag_summary.csv.
- Before/after metric deltas: outputs/before_after_deltas.csv.

## Methods
- Cleaning: Extract “Daily Data” and “Hourly Data” blocks per sheet, standardize columns, add app_id/status, and filter daily rows to midnight to build master tables.
- Change‑points: Detect the first hour per app where IVT > 0.5 and save as first_high_IVT_per_app.csv.
- Before/after: Compute means before and after each app’s first_high_IVT across IVT, idfa_ua_ratio, idfa_ip_ratio, requests_per_idfa, unique_idfas, and unique_uas, and save deltas.
- Visualization: Plot hourly IVT timelines per app and compare distributions for Valid vs Invalid groups.

## Interpretation
Flagged apps exhibit step‑changes at precise hours and maintain near‑1.0 IVT afterward, consistent with a traffic source or signature change rather than gradual ratio drift.
Across all apps, idfa_ip_ratio ≈1 and requests_per_idfa ≈1.06–1.11, so spikes are not driven by IP crowding or abnormal per‑device request intensity.

## Reproducibility
- Open notebooks/ivt_analysis.ipynb and run cells sequentially to regenerate cleaned masters, change‑points, deltas, and figures.
- Final deliverable is report/report.docx, which consolidates summary tables, visual evidence, interpretations, and recommendations.

## Figures
- Six IVT timelines (Invalid 1/2/3 and Valid 1/2/3) visualize step‑changes versus stable patterns.
- Valid vs Invalid boxplots compare IVT and key ratios to highlight group differences.

## Recommendations
- Alert when hourly IVT exceeds 0.5 and immediately review partner/source toggles at that spike hour.
- Throttle or block suspicious sources post‑spike and maintain allow‑lists for known‑good traffic to mitigate false positives.
- When raw IPs or UA strings are available, add GeoIP/ASN enrichment and UA parsing to attribute spikes to datacenters or synthetic UA clusters.

## Requirements
Create a virtual environment and install dependencies from requirements.txt to ensure consistent execution across machines.
