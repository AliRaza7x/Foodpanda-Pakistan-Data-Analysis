# 🐼 Foodpanda Pakistan — Data Analysis & Strategic Insights

---

## 📌 Project Overview

This project applies structured data analysis to a Foodpanda Pakistan dataset (n = 6,000 customer records, 2023–2025) to derive prescriptive business insights across three core dimensions: **delivery performance**, **customer retention**, and **revenue optimization**.

Using Python-based exploratory data analysis and an interactive Power BI dashboard, the project translates raw transactional data into actionable recommendations that Foodpanda Pakistan can implement to improve operational efficiency, reduce customer attrition, and maximise revenue potential.

---

## ❓ Business Questions

| # | Question | Domain |
|---|---|---|
| Q1 | How do delivery outcomes (Delivered, Cancelled, Delayed) vary across cities, order frequency, and food categories — and what operational changes can Foodpanda implement to reduce delays and cancellations? | Delivery Performance & Customer Satisfaction |
| Q2 | Which customer segments based on age, loyalty points, and order frequency are most likely to churn — and what targeted retention strategies can reduce inactivity? | Customer Churn & Retention |
| Q3 | How do payment method preferences, food category choices, and customer demographics influence revenue — and how can Foodpanda optimise promotions and pricing to maximise order value? | Revenue & Payment Optimisation |

---

## 📁 Repository Structure

```
Foodpanda-Pakistan-Data-Analysis/
│
├── data/
│   ├── Foodpanda_Analysis_Dataset.csv       # Raw dataset (6,000 records)
│
├── notebooks/
│   ├── Q1_Analysis.ipynb                    # Delivery Performance analysis (Q1)
│   ├── Q2_Analysis.ipynb                    # Customer Churn analysis (Q2)
│   └── Q3_Analysis.ipynb                    # Revenue & Payment analysis (Q3)
│
├── dashboard/
│   └── Foodpanda Analysis - Dashboards.pbix             # Power BI dashboard (3 pages)
└── README.md
```

---

## 📊 Dataset Overview

| Attribute | Details |
|---|---|
| Total Records | 6,000 |
| Time Period | August 2023 – August 2025 |
| Cities | Karachi, Lahore, Islamabad, Multan, Peshawar |
| Food Categories | Fast Food, Italian, Chinese, Continental, Dessert |
| Delivery Statuses | Delivered, Delayed, Cancelled |
| Payment Methods | Cash, Card, Wallet |
| Q2 Working Dataset | 4,533 unique customers (after cleaning) |
| Q3 Working Dataset | 4,032 orders (after excluding cancelled orders) |

**Key Variables:** `age`, `city`, `loyalty_points`, `order_frequency`, `churned`, `payment_method`, `category`, `delivery_status`, `rating`, `price`, `quantity`

> ⚠️ **Data Limitation:** The near-equal distribution across all three delivery statuses (~33% each) suggests the dataset may be synthetically generated. All findings are treated as directional rather than absolute, focusing on relative differences between segments.

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.x | Data analysis and visualisation |
| pandas | Data manipulation and cleaning |
| NumPy | Numerical operations |
| matplotlib | Static chart generation |
| seaborn | Statistical visualisations and heatmaps |
| Power BI Desktop | Interactive dashboard (3-page architecture) |
| Jupyter Notebook | Analysis environment |

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.8 or higher
- Power BI Desktop (for dashboard)

### Install dependencies

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Clone the repository

```bash
git clone https://github.com/your-username/Foodpanda-Pakistan-Data-Analysis.git
cd Foodpanda-Pakistan-Data-Analysis
```

### Run the notebooks

```bash
jupyter notebook
```

Open each notebook in the `notebooks/` folder and run all cells in order.

> **Note:** Run Q1, Q2, and Q3 notebooks independently. Each notebook performs its own cleaning pipeline on the raw dataset.

---

## 🔄 Data Preprocessing

### Base Cleaning — Applied Across All Three Notebooks
- Date columns (`signup_date`, `order_date`, `last_order_date`, `rating_date`) converted from string to datetime using `pd.to_datetime()`
- Duplicates removed via `df.drop_duplicates()`
- Null values inspected and handled
- Column names standardised using `.str.strip().str.lower().str.replace(' ', '_')`

### Q1 — Delivery Analysis
- `order_frequency` binned into Low (1–17) / Medium (18–34) / High (35–50) using `pd.cut()`
- `quarter` extracted from `order_date` using `.dt.to_period('Q')`
- `delivery_status` encoded numerically for correlation analysis: Delivered=2, Delayed=1, Cancelled=0
- Delivery outcome counts converted to percentage distributions for fair group comparison

### Q2 — Churn Analysis
- **1,467 rows removed** where `last_order_date < signup_date` (logically inconsistent) → **4,533 final records**
- Binary `churn_flag` created: `(df['churned'] == 'Inactive').astype(int)`
- `loyalty_points` (0–500) binned into Low / Medium / High using `pd.cut()` with edges `[-1, 167, 333, 500]`
- `order_frequency` (1–50) binned into Low / Medium / High using `pd.cut()` with edges `[0, 17, 34, 50]`
- `order_year` and `order_quarter` extracted for temporal trend analysis
- `tenure_days` derived as `last_order_date - signup_date`
- Cleaned dataset exported as `Foodpanda_Clean.csv` for Power BI import

### Q3 — Revenue Analysis
- Cancelled orders excluded: `df[df['delivery_status'].isin(['Delivered', 'Delayed'])]` → **4,032 final records**
- `total_revenue` calculated as `price × quantity` per row
- Year, month, and quarter extracted from `order_date` for temporal breakdown
- Revenue aggregated across 4 dimensions using `groupby` with `sum()`, `mean()`, and `count()`

---

## 📈 Key Findings

### Q1 — Delivery Performance
- Only **34.3%** of orders are successfully delivered — delays and cancellations are systemic, not city-specific
- **Karachi** achieves the highest delivery rate (36.0%) due to its role as Foodpanda's logistics hub
- **Fast Food** has the highest cancellation rate (34.2%); **Italian** has the highest delivery rate (35.5%)
- High-frequency customers achieve a **35.7% delivery rate** vs 32.9% for low-frequency — algorithmic prioritisation likely benefits frequent users
- Best quarter: **2023 Q4** (operational efficiency peak). Worst quarter: **2025 Q2** (Pakistan heatwave, 48–50°C)

### Q2 — Customer Churn & Retention
- **Baseline churn rate: 49.9%** across 4,533 customers
- **Seniors** churn most at **51.9%** — the only age group above baseline — driven by technological barriers and platform design focus on younger users
- **Low order frequency** is the strongest individual churn predictor at **51.6%** — consistent across all age and loyalty segments
- **High loyalty customers** churn above baseline at **51.0%** despite accumulating the most points — voucher redemption failures documented in Pakistan (Startup Pakistan, 2025)
- **Senior churn declining**: 54.2% (2023) → 52.5% (2024) → 49.7% (2025) — now below baseline, self-correcting
- **Adult churn rising**: 45.6% (2023) → 49.0% (2024) → 51.2% (2025) — driven by Pakistan Finance Act 2024 (55% YoY growth in salaried tax burden per FBR)
- **3-way heatmap**: Seniors have 7/9 loyalty-frequency combinations above baseline — churn is structural, not isolated

### Q3 — Revenue & Payment Optimisation
- **Total revenue: Rs. 9.69M** | **Average order value: Rs. 2,400**
- Cash, Card, and Wallet generate near-equal revenue — significant given Pakistan's 85% cash-based e-commerce market
- **Wallet spike in early 2025** driven by promotional campaign — did not convert to sustained behaviour change
- **Teenagers** are highest-revenue (Rs. 3.38M) but most volatile — peak during ICC Cricket events (2023 Q4, 2025 Q1)
- **Italian** led 2024 revenue (Rs. 1.00M); **Continental** led 2025 — reflects shift toward premium subscribers as inflation eroded mass-market purchasing power
- **Peshawar** led city revenue in 2024 (Rs. 0.99M) — lower local delivery competition gives Foodpanda stronger wallet share

---

## 💡 Recommendations

### Q1 — Delivery
- Implement preparation time SLA enforcement and auto-pause for Fast Food partners during peak hours
- Deploy AI-based demand forecasting to pre-position riders before peak periods
- Develop weather-adaptive operations protocol with reserve rider pools during extreme heat/monsoon
- Replicate Karachi's logistics model in Lahore and Peshawar through local distribution hubs

### Q2 — Churn & Retention
- **Prioritise frequency over loyalty points** — build ordering habits through 2nd and 3rd order incentives within 30 days
- **Shift focus to Adults immediately** — Senior churn is self-correcting; Adult churn is rising
- **Mid-month targeted discounts** for adults aligned with salary cycles (Finance Act 2024 impact)
- **Fix voucher redemption reliability** before scaling the loyalty program — high loyalty customers are churning because the redemption experience is broken

### Q3 — Revenue
- Pair Wallet top-up campaigns with Panda Pro enrolment to convert one-time promotions into subscription entrances
- Build event-based promotional calendars 12 months in advance (PSL, Ramzan, Azadi Month, ICC events)
- Develop Adult-specific weekday value programs with predictable daily deals
- Invest in Tier-2 city logistics (Peshawar, Multan) — strong demand with lower competition

---

## 📊 Power BI Dashboard

The dashboard is structured across **3 dedicated pages**, one per business question:

**Page 1 — Delivery Performance**
- KPI Cards: Total Orders, Delivered %, Delayed %, Cancelled %
- Delivery Status by City (grouped bar chart)
- Delivery Status by Food Category (grouped bar chart)
- Delay & Cancellation Rate by Order Frequency (line chart)
- Quarterly Delivery Trend Aug 2023 – Aug 2025 (line chart)

**Page 2 — Churn Segment Analysis**
- KPI Cards: Total Customers, Baseline Churn %, Highest Risk (Current), Highest Risk (Future)
- Churn Rate % by Age Group (bar chart with baseline)
- Yearly Churn Rate % by Age Group 2023–2025 (line chart)
- 3-way heatmaps: Combined Churn % by Loyalty × Frequency for each age group (matrix visuals)

**Page 3 — Revenue Performance**
- KPI Cards: Total Revenue, Average Order Value, Total Orders
- Total Revenue by Payment Method — Quarterly (bar chart)
- Total Revenue by Year and Category (grouped bar chart)
- Total Revenue by Quarter and Age Group (grouped bar chart)
- Total Revenue by Year and City (grouped bar chart)

> To open: Load `Foodpanda_Dashboard.pbix` in Power BI Desktop. Import `Foodpanda_Clean.csv` (Q2) and `Foodpanda_Clean_Q1.csv` (Q1/Q3) as data sources.

---

## 📚 References

| # | Citation |
|---|---|
| [1] | Bhadouriya & Sharma (2024). Zomato and Young Consumer Behavior in India. ResearchGate |
| [2] | Business Model Canvas Template (2026). Foodpanda Core Digital-Native Cohort |
| [3] | Malik, K.Z. LUMS (2025). USD 1.2 Billion Economic Impact of Foodpanda Pakistan FY2023–24. PR Newswire |
| [4] | MIT Operations Research (2018). Meal Delivery Routing Problem |
| [5] | Peracha, M. CEO Foodpanda Pakistan (2025). Executive Interview. Business Recorder |
| [6] | DHL (2024). E-commerce in Pakistan: Supply Chain and Logistics Market |
| [7] | ScienceDirect (2025). Understanding Order Cancellation Behavior in On-Demand Delivery Platforms |
| [8] | Tandfonline (2024). Customer Loyalty in Online Food Delivery in Emerging Markets |
| [9] | Trustpilot (2025). Customer Reviews — Foodpanda Pakistan |
| [10] | AM Research Journal (2026). Consumer Behavior and Food Delivery Platform Churn in Karachi |
| [11] | Karandaaz Pakistan (2024). Financial Inclusion and Consumer Payment Behavior in Pakistan |
| [12] | Ismail & Siddiqui (2019). Impact of Sales Promotion on Consumer Impulse Purchases in Karachi |
| [13] | Delivery Hero AG (2025). Full-Year 2024 Earnings Call Transcript |
| [14] | Arab News (2025). Foodpanda Doubles Pakistan Operations |
| [15] | State Bank of Pakistan (2024). Annual Report 2023–2024 |
| [16] | FBR Revenue Division Year Book 2024–25 |
| [17] | EY Pakistan Finance Bill 2024 Tax Analysis |
| [18] | Startup Pakistan (2025). Foodpanda Vouchers Not Working — Customer Complaints |
| [19] | Wikipedia — Pakistani Economic Crisis 2021–2024 |
| [20] | Pakistan Economic Survey 2024–25. Ministry of Finance |

---

## 📄 License

This project was developed for academic purposes as part of the Data Science Fundamentals course. All data used is for educational use only.

---
