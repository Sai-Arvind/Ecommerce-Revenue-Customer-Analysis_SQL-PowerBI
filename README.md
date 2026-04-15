 # 🚀 Ecommerce Analytics: Revenue Intelligence & Operational Optimization  
"64% of total revenue is driven by a single customer segment, while specific regions show disproportionately high cancellations - highlighting both growth opportunities and operational gaps.”
 
KPI Design | ETL | SQL | Power BI | Business Insights


This project transforms raw transactional data into a structured analytical framework to drive revenue growth, customer understanding, and operational efficiency.

---

### 🧩 Business Problem  

The business lacked a structured analytical framework to track performance and make data-driven decisions.

Key challenges included:

1. **Revenue Visibility:** No clear distinction btw **Gross Sales vs Net Revenue**  
2. **Customer Behaviour:** Limited insights into **who is driving the revenue (gender, age group)**  
3. **Product Performance:** No clarity on **top vs Slow moving categories**  
4. **Operational Gaps:** Hidden inefficiencies in **returns and regional cancellations**  
5. **Seasonality Trends:** No structured view of **monthly trends revenue**
6. Showcase clear growth and profitable 

---

### 🎯 Objective

To analyze e-commerce onilne data and uncover:

True revenue performance (Gross vs Net) 
Showcasing clear revenue growth and profitability drivers through data-driven analysis
Key customer segments driving growth 
Product-level revenue concentration 
Hidden operational inefficiencies (returns & cancellations) 
Seasonal demand patterns impacting revenue 

---


### 📊 Primary KPI Framework ⭐ 

| Category | KPIs | Insight | 
|----------|------|------|
| **Sales**  | Gross Sales, Net Revenue, Orders, AOV | Business health performance |
| **Customer**  | Revenue by Gender & Age | Target audience identification |
| **Product** | Category Contribution | Revenue concentration |
| **Logistics/ Operations**  | Delivery %, Cancellation Rate, Return Rate | Efficiency and cost control |


---


### 🏗️ Data Preprocessing & Preparation

1. Cleaned and processed **30,000+ transaction records** using Power Query ETL
2. Important Columns : Channels, category , Quantity, Amount, Region, Date, Order_status, Age, Gender
3. Handled **missing values** , removed **duplicates** in orders, and standardized date formats  
4. Transformed raw Csv data into structured analytical format  

---

## 🗄️ SQL Deep Analysis & Business Queries  

This section contains SQL queries used to validate KPIs, analyze trends, and uncover business inefficiencies.

### 📊 1. Revenue Metrics  

#### Total Revenue  
```sql
SELECT 
    SUM(amount) AS total_revenue
FROM ecommerce_data;

```

#### Net Revenue (Delivered Orders Only)

```sql
SELECT 
    SUM(amount) AS net_revenue
FROM ecommerce_data
WHERE order_status = 'Delivered';

```
#### Total Orders

```sql

SELECT 
    COUNT(DISTINCT order_id) AS total_orders
FROM ecommerce_data;
Average Order Value (AOV)
SELECT 
    SUM(amount) * 1.0 / COUNT(DISTINCT order_id) AS avg_order_value
FROM ecommerce_data
WHERE order_status = 'Delivered';

```

### 📈 2. Seasonality & Customer Insights

#### Monthly Revenue Trend

```sql

SELECT 
    order_month,
    SUM(amount) AS revenue
FROM ecommerce_data
WHERE order_status = 'Delivered'
GROUP BY order_month
ORDER BY order_month;
```

#### Revenue by Age Group & Gender

```sql

SELECT 
    age,
    gender,
    SUM(amount) AS revenue
FROM ecommerce_data
WHERE order_status = 'Delivered'
GROUP BY age, gender
ORDER BY age, revenue DESC;

```

### 🚚 3. Operations & Returns

#### Cancellation Rate by Region
```sql

SELECT 
    ship_state,
    COUNT(CASE WHEN order_status = 'Cancelled' THEN 1 END) * 1.0 
    / COUNT(*) AS cancellation_rate
FROM ecommerce_data
GROUP BY ship_state
ORDER BY cancellation_rate DESC;

```


### 🔄 4. Returns Analysis

#### Return Rate by Category

```sql

SELECT 
    category,
    COUNT(CASE WHEN order_status = 'Returned' THEN 1 END) * 1.0
    / COUNT(CASE WHEN order_status = 'Delivered' THEN 1 END) AS return_rate
FROM ecommerce_data
GROUP BY category
ORDER BY return_rate DESC;

```


#### Return Rate by State


```sql

SELECT 
    ship_state,
    COUNT(CASE WHEN order_status = 'Returned' THEN 1 END) * 1.0
    / COUNT(CASE WHEN order_status = 'Delivered' THEN 1 END) AS return_rate
FROM ecommerce_data
GROUP BY ship_state
ORDER BY return_rate DESC;

```
### 🧠 5. Advanced Analysis

#### Category Revenue Ranking

```sql

WITH category_revenue AS (
    SELECT 
        category,
        SUM(amount) AS revenue
    FROM ecommerce_data
    WHERE order_status = 'Delivered'
    GROUP BY category )

SELECT 
    category,
    revenue,
    RANK() OVER (ORDER BY revenue DESC) AS rank_
FROM category_revenue;

```

#### Revenue Share by Category

```sql

WITH category_revenue AS (
    SELECT 
        category,
        SUM(amount) AS revenue
    FROM ecommerce_data
    WHERE order_status = 'Delivered'
    GROUP BY category )

SELECT 
    category,
    revenue,
    revenue * 1.0 / SUM(revenue) OVER() AS revenue_pct
FROM category_revenue
ORDER BY revenue DESC;

```


---

## 🔥 What I improved 

- Fixed **SQL formatting (indentation + readability)**
- Standardized **case ('Delivered', 'Cancelled')**
- Corrected **AOV precision issue**
- Fixed **wrong ORDER BY (month vs revenue)**


> “SQL was used to perform KPI validation, trend analysis, and uncover operational inefficiencies across regions and product categories.”


---

## 2️⃣ Power Bi Visualization

#### 📊 Data Modeling 

Implemented a Star Schema for performance optimization:

Fact Table: Sales Transactions
Dimension Tables: Customers, Products, Date, Region

✅ Reduced redundancy
✅ Improved DAX performance
✅ Scalable design


## Dax Measures Utilized


#### AOV

``` power bi

AOV = 
DIVIDE(
    [Net Revenue],
    [Delivered Orders] )

```

#### Cancellation rate

```
Cancellation Rate = 
DIVIDE(
    CALCULATE(
        COUNTROWS(Ecommerce_data),
        Ecommerce_data[order_status] = "cancelled"
    ),
    COUNTROWS(Ecommerce_data) )

```

#### Ranking by Category 

```
Category Rank = 
RANKX(
    ALL(ecommerce_data[category]),
    [Category Revenue],
    ,
    DESC )

```

#### Revenue by Category 

```
Category Revenue = 
CALCULATE(
    SUM(ecommerce_data[amount]),
    ecommerce_data[order_status] = "delivered" )

```

#### Orders Delivered

```
Delivered Orders = 
CALCULATE(
    DISTINCTCOUNT(Ecommerce_data[Order ID]),
    ecommerce_data[order_status] = "delivered" )


```

#### Net Revenue

```

Net Revenue = 
CALCULATE(
    SUM(ecommerce_data[Amount]),
    ecommerce_data[order_status] = "delivered" )


```



#### Products Returned

```

Return Rate = 
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(Ecommerce_data[Order ID]),
        ecommerce_data[order_status] = "returned"
    ),
    CALCULATE(
        DISTINCTCOUNT(Ecommerce_data[Order ID]),
        ecommerce_data[order_status] = "delivered" ) )


```

#### Return %

``` 
Return_Rate_value = 
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(ecommerce_data[Order ID]),
        Ecommerce_data[Order_Status] = "Returned"
    ),
    DISTINCTCOUNT(ecommerce_data[Order ID]) )


```

#### Revenue %

```

Revenue % = 
DIVIDE(
    [Category Revenue],
    CALCULATE(
        [Category Revenue],
        ALL(ecommerce_data[category] ) ) )


```
#### No of Orders

```

Total Orders = 
DISTINCTCOUNT('Ecommerce_data'[Order ID])


```

#### Total Revenue

```
Total Revenue = SUM(ecommerce_data[Amount])


```


---


## 📊 Business Health - Executive Summary (KPIs)  

- **Gross Sales:** ₹21M  
- **Net Revenue:** ₹20M  
- **Total Orders:** 28K  
- **Average Order Value (AOV):** ₹747  
- **Return Rate:** 3.56%  



---


## 📊 Dashboard Preview  

![Dashboard](<img width="990" height="573" alt="dash" src="https://github.com/user-attachments/assets/ae55a976-2a5a-481a-b6cb-ff88cac47f9f" />)


---

# 📊 Key Insights  

### 👥 Customer Behavior  
- Women contribute **64% of total revenue**  
- Highest revenue from **age groups 36–50 and 18–25**  

---

### 🛍️ Product Performance  
- **Top Categories:**  
  - Set → ₹10.5M  
  - Kurta → ₹5.0M  
  - Western Dress → ₹3.1M  

- **Low Contribution:**  
  - Blouse, Bottom  

---

### ⚙️ Operational Efficiency  
- **92.25% orders successfully delivered**  
- **low return rate (3.56%)**  

⚠️ **High Cancellation Regions:**  
- Puducherry → 7.69%  
- Andaman & Nicobar → 6.94%  

---

### 📉 Seasonal Trends  
- Revenue declines after **Month 8 (August)**  
- Indicates missed **festive season opportunity**

---

# 💡 Business Recommendations  

1. **Regional Optimization**  
   - Improve delivery timelines in high-cancellation regions  
   - Introduce better fulfillment options (e.g., faster shipping / COD)

2. **Product Strategy**  
   - Focus inventory on **Sets & Kurtas (high revenue drivers)**  
   - Bundle low-performing items via cross-selling  

3. **Targeted Marketing**  
   - Focus on **women (36–50)** segment  
   - Launch campaigns during **festive months (Sep–Dec)**  

4. **Revenue Expansion**  
   - Use top categories to drive **cross-sell opportunities**  
   - Increase average order value through bundling  

---

# 📂 Updated Repository Structure  
vrinda-Online-store-sales-analytics/
│
├── data/
├── sql/
├── powerbi/
├── dashboard/
├── docs/
├── README.md
└── .gitignore

---


### ⚙️ Tech Stack  

| Tool | Purpose ⭐ |
|------|-----------|
| **Excel** → | ETL, Power Query, Data Cleaning, Transformation |
| **SQL** → | Deep analysis, Aggregations, calculations & segmentation |
| **Power BI** → | Data modeling, Dax measures, Dashboards, Charts & visualization |

---

# 👨‍💻 About Me  

Hi, I’m **A. Sai Arvind**, a Data Analyst focused on building scalable, performance-driven analytical solutions.  

📧 saiarvind5081@gmail.com  
🔗 LinkedIn  
🔗 GitHub  

---

⭐ If you found this project useful, consider giving it a star!
