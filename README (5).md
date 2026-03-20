# 📺 StreamIndia — OTT Subscriber Churn Intelligence System

> An end-to-end data analytics pipeline simulating a real Indian OTT platform — from synthetic data generation to SQL analysis, data cleaning, and a Power BI dashboard — designed to uncover why subscribers churn and what keeps them engaged.

---

## 📌 Project Overview

**StreamIndia** is a fully self-built analytics project that simulates the subscriber ecosystem of an Indian OTT platform. Rather than using a pre-existing dataset, this project **architects the data from scratch** using real Indian market logic — state-wise user distribution, UPI-dominant payment behaviour, and behavioural churn signals rooted in actual OTT industry patterns.

The pipeline answers one core business question:

> *"Who is churning, why are they churning, and what does their behaviour look like before they leave?"*

---

## 🗂️ Project Structure

```
ott-churn-analysis/
│
├── StreamIndia_Data_Generation.ipynb   # Phase 1 : Synthetic dataset creation
├── ott_churn_data_cleaning.ipynb       # Phase 2 : Data cleaning & preprocessing
├── ott_churn.sql                       # Phase 3 : SQL business analysis (10 queries)
├── StreamIndia_Report.pbix             # Phase 4 : Power BI dashboard (3 pages)
│
├── data/
│   ├── subscribers.csv                 # 15,000 rows — core user profiles
│   ├── watch_history.csv               # 854,184 rows — viewing sessions
│   ├── payments.csv                    # 223,480 rows — monthly billing records
│   └── support_tickets.csv            # 9,529 rows — customer support interactions
│
└── README.md
```

---

## 🔧 Tech Stack

| Tool | Purpose |
|------|---------|
| Python (Pandas, NumPy, Faker) | Data generation & cleaning |
| SQL (MS SQL Server / T-SQL) | Business analysis queries |
| Power BI Desktop | Dashboard & visualisation |

---

## 📊 Dataset Summary

| Table | Rows | Description |
|-------|------|-------------|
| `subscribers.csv` | 15,000 | User profiles with plan, device, state, age, churn flag |
| `watch_history.csv` | 854,184 | Individual viewing sessions with genre, duration, completion |
| `payments.csv` | 223,480 | Monthly payment attempts with status and method |
| `support_tickets.csv` | 9,529 | Customer complaints with issue type and resolution |

**Key Stats:**
- 📉 Churn Rate: **27.17%**
- 💰 Avg Monthly Revenue: **₹372 per subscriber**
- 🎬 Avg Watch Time: **91.9 mins per session**
- ❌ Payment Failure Rate: **7.69%**
- ✅ Ticket Resolution Rate: **51.08%**

---

## 🚀 Pipeline Walkthrough

### Phase 1 — Data Generation (`StreamIndia_Data_Generation.ipynb`)

The dataset was **not downloaded — it was architected**. Four interlinked tables were generated using Python's `Faker` library and weighted probabilistic logic that mirrors real Indian OTT behaviour.

**Churn logic baked into every user:**

| Signal | Effect on Churn Probability |
|--------|----------------------------|
| Mobile plan | +10% churn risk |
| Premium plan | −10% churn risk |
| Primary device: Mobile | +5% churn risk |
| Primary device: Smart TV | −8% churn risk |
| Acquired via Paid Ad | +8% churn risk |
| Acquired via Telecom Bundle | −7% churn risk |
| Age under 25 | +5% churn risk |
| Age over 40 | −5% churn risk |

Churn probability was clamped between **5% and 85%** to prevent unrealistic extremes.

The four tables follow a **star schema** — all connected via `user_id` — exactly how a real OTT platform's database is structured.

---

### Phase 2 — Data Cleaning (`ott_churn_data_cleaning.ipynb`)

Raw generated data was cleaned and validated before analysis:

- Handled `null` values in `churn_date` for active users
- Validated date ranges and ensured no session or payment falls outside a user's active window
- Standardised column data types (dates, integers, booleans)
- Verified referential integrity across all four tables via `user_id`
- Encoded categorical columns for downstream compatibility

---

### Phase 3 — SQL Analysis (`ott_churn.sql`)

10 business-driven SQL queries written in **T-SQL** (compatible with MS SQL Server), each targeting a specific analytical question.

| # | Query | Business Question |
|---|-------|------------------|
| Q1 | Monthly Subscriber Growth vs Churn Trend | How has our subscriber base grown month over month? |
| Q2 | Revenue by Subscription Plan | Which plan drives the most revenue? |
| Q3 | Churn Rate by Subscription Plan | Which plan has the highest churn? |
| Q4 | Top 5 States by Revenue | Where are our most valuable subscribers located? |
| Q5 | Payment Failure Analysis | How much revenue is lost to failed payments? |
| Q6 | Customer Lifetime Value by Acquisition Channel | Which channel brings the most loyal users? |
| Q7 | Cohort Retention Analysis | What % of users are still active after 1, 2, 3 months? |
| Q8 | User Engagement vs Churn | Does watch time affect churn? |
| Q9 | Issue Type vs Churn Rate | Which support issues most predict churn? |
| Q10 | Language Preference vs Churn Rate | Are regional content consumers more loyal? |

**Notable SQL techniques used:** CTEs, Window Functions (`SUM OVER`, `COUNT OVER`), `DATEDIFF`, `DATEFROMPARTS`, `COALESCE`, `ISNULL`, cohort-based conditional aggregation.

---

### Phase 4 — Power BI Dashboard (`StreamIndia_Report.pbix`)

A 3-page interactive dashboard built in Power BI Desktop.

#### Page 1 — Executive Overview
- KPI cards: Total Subscribers (15K), Active Users (11K), Churned Users (4K), Churn Rate (27.17%), Monthly Revenue (₹6M)
- Subscriber growth trend line (Jan 2022 — Dec 2024)
- Revenue split by subscription plan (donut chart)
- Active vs Churned subscriber base (donut chart)
- Churn distribution by plan (bar chart)

#### Page 2 — Churn & Payment Deep Dive
- Churn by acquisition channel (Paid Ad and Social Media lead churn)
- Churn by primary device (Mobile dominates at 55.84%)
- Churn by user tenure buckets (0–3M, 3–6M, 6–12M, 12M+)
- Churn by city tier (Tier 2 highest)
- Payment success vs failure volume
- Payment failure rate by subscription plan

#### Page 3 — User Engagement & Support Intelligence
- Content consumption by type (Web Series 40%, Movies 35%)
- Support ticket volume by issue type
- Ticket resolution status breakdown (Resolved 51%, Unresolved 30%, Escalated 19%)
- Monthly revenue by language preference
- Top states by monthly revenue (Maharashtra leads at ₹0.80M)
- Most watched genres (Drama, Comedy, Action top 3)
- Avg watch duration: Active (100 mins) vs Churned (25 mins)
- Watch sessions: Active (0.77M) vs Churned (0.09M)

---

## 💡 Key Insights

- **Mobile plan subscribers** have the highest churn (1,775 users) — they are price-sensitive and often subscribe for a single show.
- **Paid Ad** and **Social Media** acquisition channels drive the most churn — these users came for a discount, not out of loyalty.
- **Churned users watch 4× less** than active users (25 mins vs 100 mins avg session duration) — low engagement is the clearest churn predictor.
- **Payment failure rate for churned users is 5× higher** (25% vs 5%) — failed payments frequently precede cancellation.
- **Telecom Bundle users churn the least** — their subscription is tied to their phone plan, creating strong retention.
- **Hindi and English language content** generate the highest monthly revenue across all user segments.
- **Maharashtra alone contributes ₹0.80M monthly** — nearly 2× the next highest state.

---

## ▶️ How to Run

### Generate the Dataset
```bash
pip install pandas numpy faker
# Run StreamIndia_Data_Generation.ipynb in Jupyter
```

### Run SQL Queries
```sql
-- Import the 4 CSVs as tables into MS SQL Server (or Azure Data Studio)
-- Then execute ott_churn.sql
```

### Open the Dashboard
```
Open StreamIndia_Report.pbix in Power BI Desktop
Refresh data source paths to point to your local CSV files
```

---

## 📁 Data Dictionary

### `subscribers.csv`
| Column | Type | Description |
|--------|------|-------------|
| user_id | string | Unique user identifier (USR000001–USR015000) |
| age | int | User age (18–55) |
| gender | string | Male / Female / Other |
| state | string | Indian state (15 states, weighted by population) |
| city_tier | string | Tier 1 / Tier 2 / Tier 3 |
| registration_date | date | Date of subscription signup |
| subscription_plan | string | Mobile / Basic / Standard / Premium |
| monthly_price_inr | int | ₹149 / ₹299 / ₹499 / ₹799 |
| primary_device | string | Mobile / Smart TV / Laptop / Tablet / Fire Stick |
| acquisition_channel | string | Organic Search / Social Media / Referral / Paid Ad / Telecom Bundle / App Store |
| num_profiles | int | Number of profiles under the account (1–5) |
| language_preference | string | Hindi / English / Marathi / Tamil / Telugu / Bengali / Kannada |
| auto_renewal | int | 1 = enabled, 0 = disabled |
| churned | int | 1 = churned, 0 = active |
| churn_date | date | Date of churn (null if active) |

### `watch_history.csv`
| Column | Type | Description |
|--------|------|-------------|
| session_id | string | Unique session ID (SES + 6 digits) |
| user_id | string | Foreign key to subscribers |
| watch_date | date | Date of the viewing session |
| genre | string | Drama / Comedy / Action / Thriller / Romance etc. |
| content_type | string | Movie / Web Series / Short Film / Live Sports / Documentary |
| language_watched | string | Language of content consumed |
| watch_duration_mins | int | Session length in minutes |
| device_used | string | Device used for this session |
| completed | int | 1 if duration > 60 mins, else 0 |

### `payments.csv`
| Column | Type | Description |
|--------|------|-------------|
| payment_id | string | Unique payment ID (PAY + 7 digits) |
| user_id | string | Foreign key to subscribers |
| payment_date | date | Monthly billing date |
| amount_inr | int | Amount charged |
| payment_method | string | UPI / Credit Card / Debit Card / Net Banking / Wallet |
| payment_status | string | Success / Failed |
| plan_at_payment | string | Plan active at time of payment |

### `support_tickets.csv`
| Column | Type | Description |
|--------|------|-------------|
| ticket_id | string | Unique ticket ID (TKT + 6 digits) |
| user_id | string | Foreign key to subscribers |
| ticket_date | date | Date ticket was raised |
| issue_type | string | Buffering Issue / Payment Failed / Login Problem / Cancellation Request etc. |
| resolution_status | string | Resolved / Unresolved / Escalated |
| response_time_hrs | int | Support response time in hours (1–72) |

---

## 👩‍💻 Author

**Shreya Vijay Jadhav**
Data Analyst | [GitHub](https://github.com/shreyavijayjadhav3)

---

*This project was built entirely from scratch — dataset, logic, queries, and dashboard — as a portfolio demonstration of end-to-end data analytics skills applied to a real-world Indian OTT business context.*
