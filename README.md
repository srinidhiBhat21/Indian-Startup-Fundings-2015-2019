# Indian-Startup-Fundings-2015-2019 | Power BI🚀 

A 2-page interactive Power BI dashboard analysing 3,037 real startup funding deals in India from 2015 to 2019. It shows where the money went, which startups raised the most, and which investors were most active.

📌 Table of Contents
Business Questions
Dataset
Data Cleaning
Data Model
DAX Measures
Dashboard Pages
Key Insights
What I Learned

🎯 Business Questions
How much funding did Indian startups raise, and how did it change over time?
Which sectors and cities attracted the most capital?
What is the split between seed/angel and later-stage funding?
How big is a typical deal?
Which startups raised the most, and which investors were the most active?

📂 Dataset
	
Source	Indian Startup Funding (Kaggle), compiled from Trak.in
Period	Jan 2015 – Dec 2019 (5 full years)
Rows	3,037 funding deals · 2,327 startups · 5,800+ deal–investor links
Fields	Date, Startup, Industry, Sub-vertical, City, Investors, Investment Type, Amount (USD)

⚠️ Trak.in recorded far fewer deals after 2017 (310 in 2018, 111 in 2019). The drop reflects less complete tracking, not a real slowdown in funding.

🧹 Data Cleaning

The raw file is notoriously messy. Cleaning was done in Python (scripts/clean_data.py) and Power Query.

Problem	Fix
112 city spellings (Bangalore, Bengaluru, Gurgaon, Gurugram, Bangalore / SFO…)	Standardised to one name per city
55 investment-type variants (Seed/ Angel Funding, SeedFunding, PrivateEquity…)	Grouped into 10 funding stages
821 free-text industry labels	Mapped to 11 sectors using keyword rules
Broken dates (05/072018, 12/05.2015, 22/01//2015)	Repaired and parsed
Encoding junk (\xc2\xa0, \xe2\x80\x99) in names and amounts	Stripped
Duplicate startup names (Ola Cabs/Olacabs, Oyo Rooms/OYO, Pvt. Ltd suffixes, case variants)	Merged
Amount text like undisclosed, N/A, 14,342,000+	Converted to numbers; non-numeric values set to blank
11 deals recorded in INR instead of USD (e.g. Rapido listed as $3.9B instead of ₹391 Cr ≈ $55M)	Checked each against news reports and corrected; flagged in a Data Note column
Comma-separated investor lists	Split into a deal ↔ investor bridge table (one row per investor per deal)
7 deals from Jan 2020 (partial year)	Excluded so every year in the data is complete

The rupee fix alone removed about $5B of false funding from 2019.

Deals: date, startup, sector, city, state, funding stage, amount, plus Power Query columns for Year, Quarter, Deal Size band and Amount Disclosed flag
DealInvestors: bridge table, so investors can be ranked and clicked to filter deals
Sort-by-column used for Quarter, Funding Stage and Deal Size so charts display in logical order

📐 DAX Measures
Measure	DAX
Total Funding	SUM(Deals[Amount USD])
Deals	COUNTROWS(Deals)
Startups Funded	DISTINCTCOUNT(Deals[Startup])
Median Deal Size	MEDIAN(Deals[Amount USD])
Disclosed Deals %	DIVIDE(COUNT(Deals[Amount USD]), [Deals])
Investor Deals	DISTINCTCOUNT(DealInvestors[Deal ID])
Funding Share %	DIVIDE([Total Funding], CALCULATE([Total Funding], ALLSELECTED()))

📊 Dashboard Pages

Filters on both pages: Year · Sector · City · Funding Stage (synced across pages)

Page 1: Funding Overview

Funding Overview

KPI cards: Total Funding · Deals · Startups Funded · Median Deal Size · % Deals Disclosed
Combo chart: funding ($) and number of deals by quarter
Donut: deals by funding stage
Bar charts: funding by sector · top 10 startup hubs (cities)
Column chart: deals by cheque size
Page 2: Startups & Investors

Startups & Investors

Top 10 most-funded startups
Top 10 most active investors (by number of deals)
Treemap: deal count by sector
Deal Explorer table: every deal (date, startup, sector, city, stage, amount, investors). Click any bar to filter it.
💡 Key Insights
KPI	Value
Total disclosed funding	$32.45 B
Funding deals	3,037
Startups funded	2,327
Median deal size	$1.7 M
Deals with amount disclosed	68%

🏙️ Bengaluru dominates: $14.4B raised, more than Mumbai, Gurugram and Delhi combined. The top 3 cities account for 74% of all funding.
🛒 E-commerce was the funding magnet with $11.6B, followed by consumer internet ($6.3B), SaaS & technology ($3.8B) and fintech ($3.7B).
💰 Money is extremely concentrated: Flipkart ($4.76B), Paytm ($3.35B) and Ola ($2.05B) together raised about 31% of all disclosed funding.
🌱 Half of all deals are seed/angel (51%), but most of the money comes from large private-equity rounds (45% of deals).
📏 Median cheque is just $1.7M, while only 65 deals crossed $100M. The average is pulled far above the median by a few mega-rounds.
📈 2017 was the peak year ($10.4B), driven by SoftBank's mega-rounds in Flipkart ($2.5B) and Paytm ($1.4B).
🤝 Most active investors: Sequoia Capital (119 deals), Accel Partners (92), Blume Ventures (61), Kalaari Capital (60) and SAIF Partners (56).

📚 What I Learned
Real-world data needs validation, not just cleaning. Cross-checking outliers against news sources caught rupee amounts in the dollar column that would have inflated 2019 by about $5B.
Median vs average: with heavy-tailed data like startup funding, the median gives a far more honest picture of a "typical" deal.
Bridge tables handle many-to-many data (deals ↔ investors) cleanly, and both-way filtering lets users click an investor to see their deals.
RANKX with ALLSELECTED makes Top-N charts that respect every slicer, without hard-coded visual filters.
Always check data coverage before reading trends: the 2018–19 "decline" comes from less complete tracking, not from the market.

🛠 Tools

Power BI Desktop · Power Query (M) · DAX · Python (pandas) · Data Cleaning & Validation
