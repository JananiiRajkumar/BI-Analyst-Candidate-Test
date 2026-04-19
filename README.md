# PeakMedia BI Analyst Technical Assessment
**Candidate:** Jananii  Packirisamy
**Role:** BI Analyst  
**Date:** April 2025

---

## 1. Data Sources
Three raw files were provided covering Q1 2025:
- **trackAff_feed.csv** — Daily aggregated performance data from TrackAff platform. Covers operators NovaBet, SpinZone, and LuckyEdge.
- **affMatrix_feed.csv** — Player-level data from AffMatrix platform. Covers operators RoyalPlex, BlazeWin, and StarStake.
- **campaign_data.csv** — Internal daily campaign performance for three campaigns.

---

## 2. Data Quality Issues Found

### affMatrix_feed
- 6 rows with null player_id — removed as these records were unusable
- 2,693 rows with null product column — retained as players are otherwise valid
- 2,051 rows with null ftd_date — retained as these represent non-converting registrations, which are valid business data
- tracking_id and card_id imported as exponential notation — resolved by setting data type to Text in Power Query before type conversion

### trackAff_feed
- Mixed date formats — some dates in DD/MM/YYYY and some in MM/DD/YYYY format. Resolved using Power Query custom column with try/otherwise logic to handle both formats
- 3 rows with null tracking_id — removed
- 2 rows with null registrations, ftds, and total_commission — removed
- Data extends to December 2025 while other files only cover Q1 2025 — flagged as anomaly, no action taken as Q1 filter applied in report

### campaign_data
- No significant data quality issues found

---

## 3. Tracking ID Decode
The brief noted the tracking ID is a structured, meaningful number. Analysis revealed the following structure:

**Format:** `YYMMDD` + `card_id` + `3-digit suffix`

Example: `250223401001`
- `25` = Year 2025
- `02` = Month February
- `23` = Day 23rd
- `401` = card_id
- `001` = unknown suffix (not validated against any column — excluded from model)

The card_id embedded in positions 7-9 was validated by cross-referencing against the card_id column in affMatrix_feed — it matched exactly across all rows.

**campaign_id** was derived from the first digit of card_id:
- TrackAff cards: 1xx = Campaign 1, 2xx = Campaign 2, 3xx = Campaign 3
- AffMatrix cards: 4xx = Campaign 1, 5xx = Campaign 2, 6xx = Campaign 3

This mapping was validated by checking operator names consistently grouped by this derivation across all rows. This was the key join between campaign_data and both feed files.

---

## 4. Data Model Design

A star schema was implemented with the following tables:

### Dimension Tables
- **Dim_Date** — Calculated DAX table covering Q1 2025 (Jan 1 to Mar 31) with Year, Month, MonthName, Week, Weekday columns. Marked as date table. Auto date/time disabled.
- **Dim_Campaign** — 3 rows manually created using DATATABLE: campaign_id and campaign_name
- **Dim_Card** — Unified card dimension combining card_ids from both platforms with campaign_name derived from first digit of card_id

### Fact Tables
- **Fact_CampaignDaily** — from campaign_data.csv
- **Fact_TrackAff** — from trackAff_feed.csv with campaign_id added
- **Fact_PlayerMatrix** — from affMatrix_feed.csv with campaign_id added

### Relationships
All relationships are One-to-Many with single cross-filter direction:
- Dim_Date → all three fact tables on date columns
- Dim_Campaign → all three fact tables on campaign_id
- Dim_Card → both feed fact tables on card_id

### Currency Note
affMatrix reports in EUR and USD. trackAff reports in GBP and EUR. For this assessment commission values are treated as nominal figures without currency conversion. In a production environment a currency conversion table with daily exchange rates would be required for accurate cross-platform comparison.

---

## 5. DAX Measures

### Foundation Measures
- **Total Commission** — SUMX across both fact tables
- **Total Registrations** — SUM from trackAff + COUNTROWS from affMatrix
- **Total FTDs** — SUM from trackAff + COUNTROWS of non-blank ftd_date from affMatrix
- **Total Spend** — SUM of spend_gbp from campaign_data
- **Total Clicks** — SUM of clicks from campaign_data

### Ratio Measures
- **Campaign ROI** — DIVIDE(Total Commission, Total Spend)
- **Click to Reg %** — DIVIDE(Total Registrations, Total Clicks)
- **Reg to FTD %** — DIVIDE(Total FTDs, Total Registrations)
- DIVIDE used throughout to handle division by zero safely

### Time Intelligence Measures
- **Commission Last Month** — CALCULATE with DATEADD(-1, MONTH)
- **Commission MoM %** — percentage change vs last month
- **Commission Last Week** — CALCULATE with DATEADD(-7, DAY)
- **Commission WoW %** — percentage change vs last week

---

## 6. Report Design

### Page 1 — Q1 Overview
- KPI cards: Total Commission, Total FTDs, Total Registrations, Total Spend
- Bar chart: Commission by Campaign
- Line chart: Monthly Commission Trend
- Campaign slicer for dynamic filtering

### Page 2 — Campaign Analysis
- Campaign ROI bar chart
- Campaign Funnel Metrics table: Clicks, Registrations, FTDs, ratios
- Card Performance table sorted by commission descending
- Month on Month Commission matrix

---

## 7. Key Findings

- **Champions Sports Promo** generated the highest commission at £5M — the top performing campaign
- **TrackAff platform** cards significantly outperform AffMatrix cards — top 9 cards by commission are all TrackAff
- **Card 301** (LuckyEdge, Champions Sports Promo) is the single best performing card at £1.7
- M commission

---

## 8. Known Limitations

- Currency not normalised across platforms — commission totals are nominal mixed-currency figures
- Card ranking across both platforms has filter context limitations in the current model — a surrogate key approach in Power Query would resolve this with more time
- trackAff data extends beyond Q1 2025 — only Q1 data used in this report
- product column in affMatrix is null for 66% of players — product-level analysis not possible without better data capture

---

## 9. What I Would Do With More Time

- Build a currency conversion table with daily GBP exchange rates for accurate cross-platform commission comparison
- Resolve the unified card dimension using a surrogate key in Power Query
- Add RLS (Row Level Security) by operator for a production environment
- Automate data refresh via Power BI Service
- Add player lifetime value analysis using registration and FTD dates in affMatrix

---

## 10. AI Usage

Claude (Anthropic) was used as a support tool during this assessment for:
- Sense-checking data quality findings
- Troubleshooting DAX measure logic
- README structure and documentation

All analytical decisions, model design, and report design were my own. The conversation log is included in this repository as required by the brief.

---
