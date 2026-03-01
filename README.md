# Justlife Customer Analytics & Growth Strategy

**Business Intelligence Case Study | Power BI • Advanced DAX • Data Modeling**

---

## 📊 Project Overview

Analyzed 989,173 bookings worth $3.46M in GMV from a home services marketplace to identify high-impact growth opportunities through customer retention and cross-category expansion strategies.

**Key Business Questions:**
- What drives customer lifetime value in a multi-service marketplace?
- Where should we allocate marketing spend for maximum efficiency?
- How can we maximize revenue from existing customers?

---

## 🎯 Key Findings

| Metric | Finding | Business Impact |
|--------|---------|-----------------|
| **Repeat Customer Value** | 13.7x higher than one-time customers ($59 vs $4) | Focus retention over acquisition |
| **Cross-Category Premium** | 507% higher LTV ($81.56 vs $13.44) | **$197K+ incremental revenue opportunity** |
| **Customer Retention** | 62.21% return for 2+ bookings | Strong foundation to build on |
| **Geographic Efficiency** | Dubai 84.7% GMV vs 80.7% spend | Reallocation opportunity in secondary markets |
| **Marketing Concentration** | Google + Facebook = 93.8% of spend | Channel diversification opportunity |

---

## 🔍 Analysis Scope

### Dataset
- **Period:** May 2015 - November 2025 (3,866 days)
- **Transactions:** 989,173 bookings across 4 service categories
- **Customers:** 90,281 unique users
- **Revenue:** $3.46M GMV, $3.06M Net Revenue (11.55% discount rate)
- **Marketing:** $111K spend across 8 digital channels, 7 cities

### Data Sources
- **Appointment Table:** Booking-level transactional data (989,173 rows)
- **Marketing Table:** Daily aggregated marketing spend (768,797 rows)

---

## 📈 Power BI Implementation

### Data Model Architecture
**Star Schema Design:**
- Created **DimDate** table using DAX (dynamic date range from data)
- Built **DimCity** dimension table to enable cross-table filtering
- Established relationships: DimDate → Appointment/Marketing, DimCity → Appointment/Marketing
- Added **TopN_Values** parameter table for dynamic filtering

### Advanced DAX Measures (20+ Measures)

**1. Ranking (Requirement 1)**
```dax
Category GMV Rank = 
IF(
    ISINSCOPE(Appointment[Service_Category]),
    RANKX(ALLSELECTED(Appointment[Service_Category]), [Total GMV], , DESC)
)
```

**2. Time Intelligence (Requirement 2)**
```dax
GMV MoM % = 
VAR CurrentGMV = [Total GMV]
VAR PriorGMV = CALCULATE([Total GMV], DATEADD(DimDate[Date], -1, MONTH))
RETURN IF(PriorGMV <> 0, DIVIDE(CurrentGMV - PriorGMV, PriorGMV))

GMV 30D Rolling Avg = 
CALCULATE([Total GMV], DATESINPERIOD(DimDate[Date], MAX(DimDate[Date]), -30, DAY)) / 30
```

**3. Contribution Analysis (Requirement 3)**
```dax
Category GMV % = 
DIVIDE([Total GMV], CALCULATE([Total GMV], ALLSELECTED(Appointment[Service_Category])))
```

**4. Derived Metrics (Requirement 4)**
- Net Revenue = [Total GMV] - [Total Discount]
- Discount Rate = DIVIDE([Total Discount], [Total GMV], 0)
- Bookings per Customer = DIVIDE([Total Bookings], [Unique Customers], 0)
- GMV to Spend Ratio (directional efficiency metric)

**5. Dynamic Context-Aware (Requirement 5)**
```dax
Show in TopN = 
VAR SelectedN = SELECTEDVALUE(TopN_Values[TopN], 5)
VAR CurrentRank = [Category GMV Rank]
RETURN IF(CurrentRank <= SelectedN, 1, 0)
```

### Data Quality & Transformations (Power Query)
- Fixed date format mismatches between tables
- Changed Marketing Spend_USD from Whole Number to Decimal (corrected $6K discrepancy)
- Created Booking_Date column (date only, removed time component) for proper relationship
- Replaced 84 null values in Marketing[city] with 'Unknown'

---

## 💡 Strategic Insights Delivered

### Customer Segmentation Analysis
- **37.79% one-time customers** (34,119) - Re-engagement target
- **62.21% repeat customers** (56,162) - Loyalty base
- **21.65% power users** (11+ bookings) - Core revenue driver
- **51.7% single-category loyal** among repeat customers - Cross-sell opportunity

### Cross-Category Behavior
- Customers using 2+ categories: $81.56 average LTV
- Single-category customers: $13.44 average LTV
- **507% value increase** from cross-category adoption
- **29,014 customers** identified for cross-sell campaigns

### Marketing Efficiency
- Dubai: 84.7% GMV vs 80.7% spend (efficient)
- Abu Dhabi: 12.2% GMV vs 13.6% spend (slight over-invest)
- Sharjah: 2.2% GMV vs 3.6% spend (over-invested)
- Marketing spend grew 94% YoY ($3,331/month to $6,460/month)

### Discount Strategy
- Discount rates range: 6.9% (Healthcare) to 13.5% (Salon at Home)
- Correlation with volume: 0.46 (moderate, not strong)
- Opportunity to test discount reduction on high-rate categories

---

## 📊 Dashboard Features

### Executive KPIs
- Business Health: GMV, Net Revenue, Total Bookings
- Customer Quality: Repeat Rate, Multi-Category Rate
- Marketing Efficiency: Spend trends, Geographic ROI

### Interactive Elements
- Dynamic Top-N filtering (Top 3/5/10/All)
- City and service category slicers
- Drill-through to customer segment details
- MoM% growth indicators

### Visualizations
- KPI cards with conditional formatting
- Trend lines with 30-day rolling averages
- Geographic performance matrices
- Customer segmentation funnel
- Channel spend distribution charts

---

## 💼 Business Recommendations

### Immediate Actions (30 Days)
1. Launch cross-sell campaign for 29,014 single-category repeat customers (20% discount offer)
2. Re-engagement sequence for 34,119 one-time customers (Day 7/14/21 automated campaigns)
3. Test Salon at Home discount reduction (13.5% → 10%)

### Strategic Initiatives (90 Days)
1. Reallocate 1% marketing spend from Sharjah to Dubai
2. Run structured TikTok/Snapchat test (5% budget, 60 days)
3. Build cross-category journey tracking analytics

**Projected Impact:** $197K+ incremental GMV from cross-sell alone

---

## 🛠️ Technical Skills Demonstrated

### Power BI
- Star schema data modeling with dimension tables
- Advanced DAX (RANKX, CALCULATE, DATEADD, ALLSELECTED)
- Time intelligence functions (MoM%, rolling averages)
- Dynamic parameters and context-aware measures
- Performance optimization (removed redundant calculations)

### Data Analysis
- Customer cohort segmentation
- Lifetime value modeling
- Marketing attribution (directional efficiency)
- Correlation analysis
- Geographic performance benchmarking

### Business Intelligence
- Translating complex data into executive-ready insights
- Quantified business impact recommendations
- Strategic prioritization framework
- KPI design and metric definition

---

## 🎓 Key Learnings

### Analytical Insights
- Retention economics outperform acquisition (13.7x value multiplier)
- Cross-category adoption is highest-leverage growth mechanism (5.1x)
- Geographic efficiency reveals reallocation opportunities
- Discount intensity doesn't guarantee volume (0.46 correlation)

### Technical Challenges Solved
- Resolved granularity mismatch between daily aggregate and transactional data
- Built shared dimensions for cross-table filtering
- Corrected data type issues causing calculation errors
- Implemented dynamic ranking with proper filter context

---

## 📝 Use Case

This project demonstrates:
- End-to-end Power BI development from raw data to executive dashboard
- Advanced DAX proficiency across all five required categories
- Customer analytics in multi-service marketplace context
- Strategic business recommendations with quantified impact

**Relevant for:** Business Intelligence Analyst, Power BI Developer, Data Analyst roles

---

## 🔗 Connect

**Author:** Gagan Negi 
**Email:** gagan.negi66@gmail.com

---

## 📄 Notes

- This is a case study project for business intelligence analyst assessment
- Data has been anonymized for privacy
- All findings and recommendations are based on provided sample data

---

## 🙏 Acknowledgments

Case study provided by Justlife for recruitment assessment purposes.

---
## Data Link
https://docs.google.com/spreadsheets/d/15_H9wBKzz0GQOfRV_G0TkAEmOyOOLh_m/edit?usp=drive_link&ouid=102916501092487884110&rtpof=true&sd=true
