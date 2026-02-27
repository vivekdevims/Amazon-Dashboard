# 📊 Amazon Sales Analytics Dashboard (Power BI)

## 🔹 Project Overview

This project is a multi-page Power BI dashboard built using Amazon-style e-commerce sales data.  
The objective of this dashboard is to analyze business performance across revenue, products, and operational risk.

The dashboard contains **3 main pages**:

- Executive Sales Overview  
- Product Performance Analysis  
- Low Performing Products – Root Cause & Action  

This project demonstrates strong skills in:

- Data Modeling  
- DAX Measures  
- Business KPI Design  
- Root Cause Analysis  
- Executive Reporting  

---

# 📁 Dataset Overview

The dataset consists of 9 relational tables:

| Table Name  | Description |
|-------------|------------|
| customers   | Customer information |
| orders      | Order-level data |
| order_items | Product-level transaction data |
| products    | Product details |
| category    | Product category |
| sellers     | Seller information |
| payments    | Payment details |
| shipping    | Delivery & return data |
| inventory   | Stock availability |

---

# 🏗️ Data Model (Star Schema Design)

The model follows a **Star Schema** approach.

## 🔹 Fact Table

- `order_items` (Core transactional table)

## 🔹 Dimension Tables

- customers  
- products  
- category  
- sellers  
- payments  
- shipping  
- inventory  
- DateTable (custom created)

---

## 🔗 Relationships

| From | To | Relationship |
|------|----|-------------|
| customers[customer_id] | orders[customer_id] | 1:M |
| orders[order_id] | order_items[order_id] | 1:M |
| products[product_id] | order_items[product_id] | 1:M |
| category[category_id] | products[category_id] | 1:M |
| orders[order_id] | payments[order_id] | 1:M |
| orders[order_id] | shipping[order_id] | 1:M |
| products[product_id] | inventory[product_id] | 1:M |
| DateTable[Date] | orders[order_date] | 1:M |

This ensures proper filter flow from dimensions to fact table.

---

# 🛠 Tools Used

- Power BI Desktop  
- Power Query (M Language)  
- DAX (Data Analysis Expressions)  
- GitHub (Project Documentation)  

---

# ⚙️ Data Preparation Steps

1. Imported CSV files into Power BI.
2. Cleaned data in Power Query:
   - Removed null values
   - Standardized data types
   - Created Revenue column
3. Created a Date Table using DAX.
4. Built relationships in Model View.
5. Created calculated columns & measures.
6. Designed interactive dashboards.

---

# 📐 Calculated Columns Created

## 1️⃣ Revenue (order_items table)

```DAX
Revenue = order_items[quantity] * order_items[price_per_unit]
```

**Purpose:**  
Calculates revenue at transaction level.

---

## 2️⃣ Customer Frequency Segment (customers table)

Segments customers based on order count.

---

## 3️⃣ Risk Category (products table)

Categorizes products into High Risk / Low Risk based on return rate.

**Total Calculated Columns Created:** ~3–5

---

# 📊 Measures Created (Core KPIs)

---

## 🔹 Revenue Measures

### Total Revenue
```DAX
Total Revenue = SUM(order_items[Revenue])
```

### Product Revenue
```DAX
Product Revenue = SUM(order_items[Revenue])
```

---

## 🔹 Order Measures

### Total Orders
```DAX
Total Orders = DISTINCTCOUNT(orders[order_id])
```

### Units Sold
```DAX
Units Sold = SUM(order_items[quantity])
```

---

## 🔹 Customer Measures

### Total Customers
```DAX
Total Customers = DISTINCTCOUNT(orders[customer_id])
```

---

## 🔹 Return Analysis

### Returned Orders
```DAX
Returned Orders =
CALCULATE(
    DISTINCTCOUNT(shipping[order_id]),
    NOT(ISBLANK(shipping[return_date]))
)
```

### Return Rate %
```DAX
Return Rate % =
DIVIDE([Returned Orders], [Total Orders], 0)
```

---

## 🔹 Payment Risk

### Failed Payment Rate %
```DAX
Failed Payment Rate % =
DIVIDE(
    CALCULATE(
        COUNT(payments[order_id]),
        payments[payment_status] = "Failed"
    ),
    COUNT(payments[order_id]),
    0
)
```

---

## 🔹 Time Intelligence

### Revenue Previous Month
```DAX
Revenue PM =
CALCULATE(
    [Total Revenue],
    DATEADD('DateTable'[Date], -1, MONTH)
)
```

### Revenue MoM %
```DAX
Revenue MoM % =
DIVIDE(
    [Total Revenue] - [Revenue PM],
    [Revenue PM]
)
```

---

## 🔹 Bottom 10 Revenue Logic (Advanced DAX)

Used for Page 3:

```DAX
Bottom 10 Total Revenue =
VAR BottomProducts =
    TOPN(
        10,
        FILTER(
            ALL(products[product_name]),
            [Product Revenue] > 0
        ),
        [Product Revenue],
        ASC
    )
RETURN
SUMX(BottomProducts, [Product Revenue])
```

**Total Measures Created:** ~25–35 measures including:

- Revenue KPIs  
- MoM Growth  
- Return Analysis  
- Product Risk  
- Inventory Metrics  
- Payment Metrics  
- Bottom 10 Analysis  

---

# 📄 Dashboard Pages

---

## 📌 Page 1 — Executive Sales Overview

!<img width="1309" height="738" alt="page1" src="https://github.com/user-attachments/assets/5f7e3b89-e3c8-4c2a-af60-db6b6536f5b1" />


**Focus:**
- Total Revenue  
- Total Orders  
- AOV  
- Total Customers  
- Monthly Growth  
- Category Performance  

**Business Questions Answered:**
- Is revenue growing?
- Which category performs best?
- What is overall business health?

---

## 📌 Page 2 — Product Performance Analysis

!<img width="1299" height="726" alt="page2" src="https://github.com/user-attachments/assets/5b2ca1da-af5a-4546-ba56-fac847ad3cce" />


**Focus:**
- Top 10 Products by Revenue  
- Return Rate by Product  
- Revenue vs Return Risk Scatter  
- Stock Risk  
- Monthly Trend (Selected Product)  

**Business Questions Answered:**
- Which products drive revenue?
- Which products have high return rates?
- Are high revenue products risky?

---

## 📌 Page 3 — Low Performing Products (Root Cause)

!<img width="1283" height="730" alt="page3" src="https://github.com/user-attachments/assets/00dd2595-2c1c-4478-a76b-496ab313ead6" />



**Focus:**
- Bottom 10 Products by Revenue  
- Price vs Category Comparison  
- Return Rate Analysis  
- Quantity Sold  
- Action Recommendations  

**Business Questions Answered:**
- Why are products underperforming?
- Is pricing the issue?
- Is return rate high?
- Is demand low?

---

# 📈 Business Problems Solved

- Revenue concentration risk  
- Product return risk  
- Inventory imbalance  
- Payment failure risk  
- Identification of low-performing products  

---

# 🚀 Key Skills Demonstrated

- Advanced DAX (TOPN, SUMX, CALCULATE, FILTER)
- Time Intelligence (DATEADD)
- Dynamic KPI formatting
- Star schema modeling
- Executive storytelling with data

---

# 📌 Conclusion

This Power BI dashboard provides a structured and strategic analysis of Amazon-style e-commerce data, helping management make informed decisions about revenue growth, product optimization, and operational risk management.

