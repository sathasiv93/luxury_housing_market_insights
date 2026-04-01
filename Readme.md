# Luxury Housing Sales Analysis — Bengaluru

End-to-end real estate analytics project using **Python**, **BigQuery (SQL)** and **Looker Studio (Data Viz)** on a 100K+ record luxury housing dataset from Bangalore.

---

## Problem Statement

Build a complete real estate analytics solution using Python for data cleaning, load the refined dataset into BigQuery, and use Looker Studio to build an interactive dashboard. The goal is to replicate a real-world enterprise-level data pipeline and analysis environment.

---

## Dataset

- **Source:** Luxury Housing Bangalore CSV
- **Size:** 101,000 rows × 18 columns (raw) → 100,000 rows × 26 columns (cleaned)
- **Key Columns:** Property_ID, Micro_Market, Developer_Name, Ticket_Price_Cr, Configuration, Amenity_Score, Buyer_Type, Purchase_Quarter, Sales_Channel, NRI_Buyer, Buyer_Comments

---

## Project Structure

```
luxury-housing-analysis/
│
├── README.md                              ← You are here
│
├── data/
│   ├── Luxury_Housing_Bangalore.csv       ← Raw dataset
│
├── scripts/
│   └── Luxury_Housing_Cleaning.ipynb      ← Python notebook (Colab)
│
├── reports/
│   └── Insights_Report.md                 ← Key findings & insights
│
└── Luxury Housing Market Insights (2023-25).pdf
```

---

## Tools & Technologies

| Tool | Purpose |
|------|---------|
| Python (Pandas, NumPy) | Data cleaning & feature engineering |
| Google Colab | Development environment |
| Google BigQuery | SQL data warehouse |
| Looker Studio | Interactive dashboard |
| GitHub | Version control |

---

## Pipeline

```
Raw CSV → Python (Clean + Engineer) → BigQuery (SQL) → Looker Studio (Dashboard)
```

### Step 1: Python — Data Cleaning & Feature Engineering
- Removed 1,000 exact duplicate rows
- Standardized text columns: Configuration (9→3), Micro_Market (48→16)
- Cleaned Ticket_Price_Cr: removed ₹ and Cr formatting, converted to float
- Fixed 500 invalid Unit_Size_Sqft values (-1) and 5 negative prices
- Filled nulls: numeric columns with median, Buyer_Comments with placeholder
- Engineered 8 new features: Price_Per_Sqft, Price_Tier, Quarter_Label, NRI_Flag, Comment_Sentiment, Amenity_Tier, Purchase_Year, Purchase_Qtr_Num

### Step 2: BigQuery — SQL Warehouse
- Loaded cleaned data using `pandas-gbq` via authenticated Colab session
- Created `luxury_housing.luxury_housing_listings_cleaned` table
- Ran validation queries: row count, developer aggregation, micro-market summary

### Step 3: Looker Studio — Dashboard
- Connected Looker Studio directly to BigQuery
- Built 5-page interactive dashboard with filters, KPIs, and charts
- Dashboard link: [Looker Studio Report]([YOUR_LOOKER_LINK_HERE](https://lookerstudio.google.com/u/0/reporting/0c3fb1bf-ebb4-4bc6-a04a-994c62ca564f/page/p_oajy8zbc2d/edit))

---

## Data Cleaning Summary

| Issue | Count | Fix |
|-------|-------|-----|
| Exact duplicate rows | 1,000 | `drop_duplicates()` |
| Configuration casing (3BHK/3Bhk/3bhk) | 9 → 3 | `.str.upper()` |
| Micro_Market casing | 48 → 16 | `.str.title()` |
| Ticket_Price_Cr as string with ₹/Cr | 18,211 | `.str.replace()` + `pd.to_numeric()` |
| Unit_Size_Sqft = -1 | 500 | `loc[]` → NaN → median |
| Negative prices | 5 | `loc[]` → NaN → median |
| Null Unit_Size_Sqft | ~10,000 | `fillna(median)` |
| Null Ticket_Price_Cr | ~10,000 | `fillna(median)` |
| Null Amenity_Score | ~10,000 | `fillna(median)` |
| Null Buyer_Comments | ~18,000 | `fillna('No Comment')` |

---

## Features Engineered

| Feature | Method | Purpose |
|---------|--------|---------|
| Purchase_Year | `.dt.year` | Time-series filtering |
| Purchase_Qtr_Num | `.dt.quarter` | Quarterly grouping |
| Quarter_Label | String concat | Dashboard axis labels |
| Price_Per_Sqft | Price × 1Cr ÷ Sqft | Comparable pricing metric |
| Price_Tier | `pd.cut()` — 5 bins | Market segmentation |
| NRI_Flag | `np.where()` — 0/1 | NRI percentage calculation |
| Comment_Sentiment | `in sql model` with keywords | Buyer persona analysis |
| Amenity_Category | `in sql model` | Amenity correlation |

---

## Dashboard Pages

| Page | Title | Key Visuals |
|------|-------|-------------|
| 1 | Executive Overview | KPI scorecards, Top 5 builders, Configuration demand |
| 2 | Quarterly Trends | Time series by market, Builder × Quarter matrix |
| 3 | Builder Performance | Revenue bar chart, Sales channel breakdown |
| 4 | Buyer Intelligence | Buyer type × Possession, NRI split, Sentiment |
| 5 | Location & Amenity | Micro-market table, Amenity vs Price, Connectivity |

---

## How to Run

1. **Clone the repo**
   ```bash
   git clone https://github.com/YOUR_USERNAME/luxury_housing_market_insights.git
   ```

2. **Open the notebook in Colab**
   - Upload `Luxury_Housing_Bangalore.csv` to Colab
   - Run all cells in `Luxury_Housing_Cleaning.ipynb`

3. **BigQuery push** (requires GCP project)
   - Authenticate in Colab: `auth.authenticate_user()`
   - Update `PROJECT_ID` in the notebook
   - Run the BigQuery cell

4. **Looker Studio**
   - Connect to BigQuery → `luxury_housing.mart_luxury_housing_listings`
   - Build visuals using the column mappings in `reports/Insights_Report.md`

---

## Links

- **Colab Notebook:** [Open in Colab](https://colab.research.google.com/drive/1HphuT80LIn7406YBSucAQj2XxCqn9tk-)
- **Looker Studio Dashboard:** (https://lookerstudio.google.com/u/0/reporting/0c3fb1bf-ebb4-4bc6-a04a-994c62ca564f/page/p_oajy8zbc2d/edit)

---


Raphaël Hoguet

---
