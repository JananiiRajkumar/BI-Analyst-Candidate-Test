# PeakMedia — Power BI Analyst Technical Assessment

## About PeakMedia

PeakMedia is an iGaming affiliate marketing company. We drive traffic to online casino and sportsbook operators through performance marketing campaigns. When a player clicks one of our campaign links, registers with an operator, and makes their first deposit, we earn a commission.

Our data function sits at the heart of how we understand performance, attribute revenue, and make commercial decisions. This exercise is representative of the kind of work you would be doing from day one.

---

## The Scenario

You have been handed three raw data files covering Q1 2025 (January–March). They come from two different affiliate tracking platforms we work with, and our internal campaign reporting system.

Nobody has cleaned or modelled this data yet. Your job is to make sense of it, bring it together in Power BI, and turn it into something a commercial team can actually use.

---

## The Files

| File | Source | Description |
|---|---|---|
| `trackAff_feed.csv` | TrackAff (Affiliate Platform A) | Daily aggregated performance data. Covers operators NovaBet, SpinZone, and LuckyEdge. |
| `affMatrix_feed.csv` | AffMatrix (Affiliate Platform B) | Player-level data. Covers operators RoyalPlex, BlazeWin, and StarStake. |
| `campaign_data.csv` | Internal | Daily campaign performance for PeakMedia's three active campaigns. |

---

## What We Want To See

### 1. Data Cleaning & Preparation

Load all three files into Power BI using Power Query. Clean and shape the data, and document every issue you find and every decision you make about how to handle it. If you make an assumption, write it down.

We are not looking for perfection — we are looking for how you think when data does not behave.

### 2. Data Modelling

Design a star schema in Power BI that brings the three sources together into a unified, queryable model.

Show us:
- The tables you have created and how you have structured them
- How you have connected the sources (and why)
- How you have handled the date table — we expect a proper date dimension, not an auto date/time table

Think about this as if it were going into a production report used by the commercial team every day.

### 3. Analysis — DAX Measures

Using DAX, produce measures that answer the following:

- Which operator generated the most total commission over the quarter?
- Which campaign delivered the best return on spend (commission earned vs spend)?
- What is the click-to-registration ratio and the registration-to-FTD ratio, by operator and by campaign?
- Which card performed best within each campaign?
- Week-on-week and month-on-month commission change — for the business as a whole and by operator
- Are there any anomalies or data quality issues you think the business should be made aware of? Flag them clearly.

Where relevant, write your measures using `VAR / RETURN` blocks. Avoid nested function spaghetti.

### 4. Dashboard

Build a Power BI report that a non-technical commercial stakeholder can read and act on. It should cover at minimum:

- Overall Q1 commission performance with period comparisons
- Campaign ROI comparison
- Operator breakdown with click, registration, and FTD funnel metrics
- At least one dynamic element (e.g. a slicer-driven measure, a ranking visual, or a period selector)

Keep it clean and purposeful — clarity over complexity. A well-structured two-page report beats a cluttered five-page one.

### 5. README

Write a short document covering:
- What you found in the data and how you handled it
- Your data model design and the reasoning behind it
- How you structured your DAX — any patterns you followed and why
- What you would do differently with more time or access to better tooling
- Where and how you used AI tools, if at all

---

## Notes

- **The data contains issues.** Finding them is part of the exercise.
- **Operators report in different currencies.** You will need to think about how to handle this for cross-operator comparisons in your model and measures.
- **The tracking ID is a structured, meaningful number.** Understanding what it encodes will help you connect the data sources. Look carefully.
- **Use whatever tools you normally use day-to-day.** If AI is part of your workflow, that is fine — note where and how you used it, and include any relevant conversation logs in your submission.
- **Set up your own environment.** Come to the interview ready to walk us through your Power BI file live, including the data model view and your DAX measures.

---

## Submission

Create a GitHub repository and share the link by the deadline below.

Your repository should include:
- Your Power BI file (`.pbix`)
- Any supporting scripts used for data preparation (Python or SQL, if used)
- AI conversation logs if you used AI tools
- Your `README.md`

**Submission deadline:** [TO BE FILLED — 5 working days from receipt]

---

## Time Expectation

This exercise is designed to take **2–3 focused hours**. It is not intended to consume your weekend or require an elaborate production-grade system. We are looking for sound thinking, clean execution, and honest documentation — not over-engineering.

---

*If you have any questions about the brief, reply to this email and we will respond within one working day.*
