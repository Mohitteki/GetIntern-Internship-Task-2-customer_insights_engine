# E-Commerce Customer Insights Engine

- **Author:** Teki Mohit Sai Tulasi
- **Program:** GetIntern.in 8-Week Practical Data Science & ML Internship
- **Phase 1 / Week 2 Milestone Project:** Merging Tables & Calculating Business Metrics

A Pandas pipeline that joins customer and order tables, calculates core business KPIs (total spend, average order value, order volume by city) and builds a leaderboard of the top 5 highest-spending customers.

## Repository Contents
| File | Description |
|------|-------------|
| `customer_insights_engine.ipynb` | Fully executed Jupyter Notebook with visible outputs |
| `customers.csv` | Raw customers table (10,000 rows) |
| `orders.csv` | Raw orders table (20,000 rows) |
| `customer_summary.csv` | Customers with total spend, order count and AOV |
| `city_summary.csv` | Orders, revenue and AOV per city |
| `top5_customers.csv` | Top 5 customer leaderboard |
| `README.md` | Project documentation |

## Dataset
Source: *E-commerce_dataset* (Kaggle, CC BY 4.0) - https://www.kaggle.com/datasets/abhayayare/e-commerce-dataset
Only `users.csv` (renamed `customers.csv`) and `orders.csv` were used. The key column `user_id` was renamed to `customer_id` inside the notebook.

- **customers:** `customer_id, name, email, gender, city, signup_date`
- **orders:** `order_id, customer_id, order_date, order_status, total_amount`

## Business Rule
Only orders with status **`completed`** count as purchases and revenue (4,021 of 20,000 orders). Cancelled, returned, shipped and processing orders are excluded.

## Methodology

### 1. Relational Table Joins (`pd.merge`)
Both tables were joined on `customer_id`. There were no nulls or duplicates, and every order matched a customer.

| Join | Rows | Meaning |
|------|------|---------|
| Inner | 4,021 | Only customers who have completed orders |
| Left | 10,701 | All customers; 6,680 have blank order columns because they never purchased |

The left join has more rows than the 10,000 customers because customers with several orders appear once per order.

### 2. Metric Aggregation & Segmentation (`groupby` + `agg`)
- **Per customer:** total spend, number of orders, average order value (AOV)
- **Per city:** total orders, total revenue, AOV and unique customers

### 3. Cohort Filtering & Leaderboard
- Customers with zero purchases: **6,680**
- Customers kept after filtering: **3,320** (from 10,000)
- Top 5 customers ranked by total spend

## Top 5 Highest-Spending Customers
| Rank | Customer ID | Name | City | Orders | Total Spend | AOV |
|-----:|:------------|:-----|:-----|-------:|------------:|----:|
| 1 | U002814 | Patricia Olson | West Michaelville | 1 | 6360.11 | 6360.11 |
| 2 | U006469 | Audrey Ware | South Robertstad | 2 | 6341.39 | 3170.70 |
| 3 | U005921 | Stanley Wood | South Peterstad | 1 | 6195.80 | 6195.80 |
| 4 | U005803 | Alyssa Jones | Turnermouth | 1 | 6066.85 | 6066.85 |
| 5 | U003637 | Eric Hernandez | West Tonyashire | 2 | 5952.53 | 2976.26 |

## Key Observations
- Total revenue from completed orders is **2,419,712.58**, and the city-level and customer-level totals reconcile exactly.
- About two thirds of customers (66.8%) have no completed purchase.
- City names in this dataset are almost all unique, so most cities contain only a few customers. City-level totals are therefore small and should be read with care.
- The top spenders are mostly single high-value orders, not repeat buyers.

## How to Run
```bash
pip install pandas numpy jupyter tabulate
jupyter notebook customer_insights_engine.ipynb
```
Place `customers.csv` and `orders.csv` in the same folder and run all cells from top to bottom.

## Tech Stack
Python 3, Pandas, NumPy, Jupyter Notebook
