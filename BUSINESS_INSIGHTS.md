# Business Insights — Interview Brief

## Executive summary

The dashboard evaluates 25 hotel property IDs across Mumbai, Bangalore, Hyderabad, and Delhi from **1 May to 31 July 2022**. The portfolio generated **1.7088bn** in realized revenue from **134,590 bookings**. Commercial performance is steady rather than fast-growing: pricing is stable, while occupancy, cancellations, and property-level guest experience present the clearer improvement opportunities.

## Numbers to remember

| Metric | Value | What it means |
|---|---:|---|
| Revenue | 1.7088bn | Total realized revenue |
| Occupancy | 40.59% | 94,411 checked-out bookings / 232,576 capacity |
| ADR | 12.70K | Realized revenue per booking |
| RevPAR | 7.35K | Realized revenue per available room |
| Realisation | 70.15% | Share of bookings that checked out |
| Cancellation / no-show | 24.83% / 5.02% | Roughly 3 in 10 bookings did not realize |
| Average rating | 3.62 | Overall guest rating |

## Main business story

1. **Occupancy is the main lever.** Weekend occupancy reaches **51.85%**, compared with **35.92%** on weekdays—a **15.93 percentage-point gap**. Weekend RevPAR is about **9.36K** versus **6.51K** on weekdays, while ADR is nearly identical (12.72K versus 12.68K). This suggests targeted weekday demand generation is more important than broad price cuts.

2. **Luxury is the revenue engine.** Luxury hotels generate **1.053bn (61.61%)** of revenue; Business hotels contribute **656.0m (38.39%)**. The mix makes Luxury the priority segment for protecting service quality and rate realization.

3. **Mumbai leads scale, but Delhi leads capacity use.** Mumbai produces **668.6m**, around **39.1%** of total revenue. Delhi has the highest city occupancy at **42.42%**, followed by Hyderabad (40.84%), Mumbai (40.65%), and Bangalore (38.99%). This means the largest revenue market is not automatically the most efficient market.

4. **Channel differences are mostly volume-driven.** The `others` platform group contributes about **699.4m (40.9%)**, followed by `makeyourtrip` at **340.8m (19.9%)**. Across named platforms, ADR stays within roughly **12.63K–12.79K** and realization within **69.83%–70.59%**. Channel mix changes volume much more than unit economics.

5. **Revenue is stable, not accelerating.** Monthly revenue moved from **581.9m in May** to **553.9m in June**, then recovered to **572.9m in July**. July remained slightly below May, so the period does not show sustained growth.

6. **Booking leakage is material.** The model contains **33,420 cancellations** and **6,759 no-shows**. Improving policies, reminders, deposits, and re-selling workflows could raise realization without adding room capacity.

## Recommended actions

- Build weekday packages for corporate accounts, local events, and longer stays; measure success through weekday Occupancy and RevPAR.
- Investigate cancellation and no-show drivers by platform, property, room class, and lead time before changing policy portfolio-wide.
- Protect the Luxury segment's service quality because it supplies most revenue.
- Review lower-rated properties individually and compare service issues with their occupancy and cancellation patterns.
- Separate the broad `others` booking-platform group into identifiable channels if the source data permits; it currently hides the largest share of revenue.

## Likely interview questions

**What is the strongest insight?**  
The weekend/weekday gap. ADR barely changes, but occupancy rises sharply on weekends, so demand—not price—is the major reason for higher weekend RevPAR.

**Would you reduce prices to improve performance?**  
Not across the portfolio. Pricing is already consistent by day type and platform. I would first use segmented weekday offers and test whether incremental demand improves RevPAR without unnecessarily lowering ADR.

**Where would you focus first?**  
Weekday occupancy and booking leakage. Together they offer upside from both unused capacity and bookings that currently cancel or fail to show.

**Why use both ADR and RevPAR?**  
ADR shows the revenue earned per booking, while RevPAR incorporates available capacity. ADR can look healthy even when many rooms remain unused; RevPAR exposes that utilization gap.

**What is a limitation of this analysis?**  
It covers only three months and contains no cost or profit data, customer segments, booking lead time, or explicit currency metadata. The analysis supports revenue and utilization decisions, but not full profitability conclusions.

## Technical points worth mentioning

- The semantic model follows a star-style design with two fact tables and shared hotel, room, and date dimensions.
- Measures are centralized in a dedicated `key_measures` table and use filter context so every KPI responds to slicers.
- Week-over-week measures select the current week and compare it with `week_num - 1`. They should be interpreted carefully when the latest week is incomplete or when a report filter removes the prior week.
- The source value for weekdays is spelled `weekeday` in `dim_date`; standardizing it upstream would improve data quality without changing the analysis.
