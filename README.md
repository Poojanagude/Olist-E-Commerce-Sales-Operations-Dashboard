Olist E-Commerce Sales & Operations Dashboard
Author: Pooja Nagude Tool: Microsoft Power BI Dataset: Brazilian E-Commerce Public Dataset by Olist (Kaggle) Report type: 3-page interactive Power BI dashboard

1. Business Problem
Olist is a Brazilian e-commerce marketplace that connects small businesses to major online sales channels. Leadership needs a single source of truth to answer three recurring questions:

How is the business performing overall — revenue, orders, customers, and product-level trends?
Who are our customers and sellers, and how much value do they generate over time?
How well are we operationally executing — delivery speed, on-time performance, and customer satisfaction?
Without a consolidated dashboard, these answers were scattered across raw transactional tables (orders, payments, reviews, deliveries), making it slow for stakeholders to spot trends or problem areas.

2. Objectives
Consolidate order, payment, customer, seller, product, and review data into one connected data model.
Track core commercial KPIs (revenue, orders, AOV, customer count) at a glance.
Segment revenue by geography, product category, and payment method.
Measure customer value and repeat-purchase behavior.
Monitor delivery performance and its relationship to customer satisfaction (review scores).
Package all of the above into a self-service, filterable Power BI report.
3. Target Audience / Stakeholders
E-commerce operations managers — delivery performance, order status.
Marketing / growth team — customer lifetime value, repeat rate, top customers.
Category / merchandising team — top product categories and sellers by revenue.
Executive leadership — headline revenue, order, and satisfaction metrics.
4. Data Source & Model
The Olist dataset is a normalized relational set of CSVs (orders, order_items, order_payments, order_reviews, customers, sellers, products, product_category_translation, geolocation). Key relationships used in this model:

orders (1) → (many) order_items → (many-to-1) products
orders (1) → (many) order_payments
orders (1) → (1) order_reviews
orders (many-to-1) customers
order_items (many-to-1) sellers
A Date table was built and marked as the official date table to support time-intelligence and correct chronological sorting (see Known Issues, Section 8).

5. KPI Definitions
KPI	Definition	Page
Total Revenue	Sum of payment_value across all completed orders	1
Total Orders	Distinct count of order_id	1
Total Customers	Distinct count of customer_id	1, 2
Avg Order Value (AOV)	Total Revenue ÷ Total Orders	1
Avg Review Score	Average of review_score (1–5 scale)	1, 3
Repeat Customer Rate	(Customers with >1 order) ÷ (Total Customers)	2
Customer Lifetime Value (CLV)	Total Revenue generated per customer over their full order history	2
Revenue per Customer	Total Revenue ÷ Total Customers	2
Avg Orders per Customer	Total Orders ÷ Total Customers	2
Average Delivery Days	Average of (order_delivered_customer_date − order_purchase_timestamp)	3
Positive Reviews %	Orders with review_score ≥ 4 ÷ Total Reviewed Orders	3
Negative Reviews %	Orders with review_score ≤ 2 ÷ Total Reviewed Orders	3
On-Time vs Late Delivery %	Share of orders delivered on/before order_estimated_delivery_date vs after	3
6. Dashboard Structure
Page 1 — Sales & Operations Overview
Headline KPI cards (Revenue, Orders, Customers, AOV, Avg Review) plus:

Total Revenue by order month (trend)
Total Revenue by customer state (geographic breakdown)
Top 5 product categories by revenue
Payment type distribution (credit card, boleto, voucher, debit card)
Order status distribution (delivered, shipped, canceled, etc.)
Review score distribution
Filters: order year, order month, payment type, customer state, product category.

Page 2 — Sales & Customer Analysis
Repeat Customer Rate, CLV, Revenue per Customer, Avg Orders per Customer (KPI cards)
Total customers by order year-month (trend)
Top 10 sellers by revenue
Top 10 customers by revenue
Page 3 — Operations & Delivery Analysis
Average Delivery Days, Positive Reviews %, Average Review Score, Negative Reviews % (KPI cards)
Monthly delivery trend
On-time vs late delivery split
Order status distribution
7. Key Insights
Credit card is the dominant payment method, used in the large majority of transactions, followed by boleto (a Brazilian bank-slip payment method).
The overwhelming majority of orders reach "delivered" status, with cancellations and other statuses forming a small share.
Review scores skew positive, with the highest single bucket at the top of the 1–5 scale.
A small percentage of sellers and customers account for a disproportionate share of revenue — consistent with typical marketplace Pareto distribution.
Average delivery time and late-delivery rate provide a direct operational lever connected to review sentiment — orders that arrive late are a natural hypothesis for driving negative reviews (worth a dedicated correlation view — see Section 9).
8. Tools & Techniques Used
Power BI Desktop — data modeling, DAX measures, report design
Power Query — data cleaning, type transformations, merging tables
DAX — KPI measures, time intelligence, percentage calculations
Data modeling — star-schema-oriented relationships across 6+ tables
9. Known Issues & Recommended Fixes
This is a first-pass build. The following issues are documented transparently so reviewers can see both the analysis and the iteration process:

Unlicensed custom visual on Page 1 (Total Revenue by customer state) currently renders an error — needs to be swapped for a native Power BI map/bar visual.
Date/month sorting on the "Total Revenue by order_month" and "Monthly Delivery Trend" charts is currently alphabetical/categorical rather than chronological — needs a proper Date table with a numeric sort-by column.
Duplicate KPI values — Revenue per Customer and CLV currently return identical figures; the underlying DAX measures need to be differentiated (CLV should ideally use a rolling/lifetime window, not a simple average).
Formatting bug — "Avg Orders per Customer" is displaying with a currency symbol; should be formatted as a plain number.
Unlabeled boolean fields — the On-Time vs Late Delivery donut uses raw 0/1 values instead of descriptive labels ("On-Time" / "Late").
ID-based charts (Top 10 Sellers/Customers) show raw hashed IDs; consider truncating with a friendly alias or tooltip with more context.
Trailing partial-period data — the last month(s) in the customer trend drop to near-zero, likely due to an incomplete final period in the source data; recommend filtering out or footnoting incomplete periods.
10. Future Enhancements
Add a correlation view between delivery delay and review score.
Add cohort-based retention analysis (month-over-month repeat purchase).
Add a geographic map (state/city level) once the licensing issue is resolved.
Add RFM (Recency, Frequency, Monetary) customer segmentation.
Publish a live Power BI Service link with scheduled refresh.
11. How to Reproduce
1.Download the Olist dataset from Kaggle (link above).
2.Load the CSVs into Power BI Desktop via Power Query.
3.Build relationships as described in Section 
4.Create a dedicated Date table and mark it as the date table.
5.Add the DAX measures listed in Section 
6.Rebuild the three report pages per Section 
12. Screenshots
Add exported PNG screenshots of each page here (drag into an /screenshots folder in your repo) and reference them like:
Page 1:
<img width="1901" height="962" alt="image" src="https://github.com/user-attachments/assets/ef48219e-ae8c-44cb-bcbd-2e4d60cf6a43" />

Page 2:
<img width="1472" height="822" alt="image" src="https://github.com/user-attachments/assets/df79dea7-31a2-41d2-bb05-660f06d18b84" />

Page 3:
<img width="1476" height="855" alt="image" src="https://github.com/user-attachments/assets/4a1c117e-2441-4567-aefa-ef26321dd8f3" />





This project was built using the publicly available Olist Brazilian E-Commerce dataset for educational/portfolio purposes and is not affiliated with or endorsed by Olist.
