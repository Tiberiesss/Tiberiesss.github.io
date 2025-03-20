# 🛒 E-Commerce Data Analytics Project  

## 📌 Project Overview  
This project focuses on analyzing real e-commerce transaction data from **Kaggle (2010-2011)** to uncover key business insights. The goal is to transform raw data into actionable intelligence using **SQL and Power BI**, helping businesses optimize sales, customer retention, and revenue strategies.  

## 🎯 Objectives  
- **Clean and preprocess raw e-commerce data** for accurate analysis  
- **Segment customers using RFM analysis** (Recency, Frequency, Monetary)  
- **Analyze revenue trends**, best-selling products, and customer behavior  
- **Identify peak sales hours** and **return rates**  
- **Visualize key metrics** with an interactive **Power BI dashboard**  

## 🛠 Tools & Technologies  
- **Excel** → Initial data cleaning and structuring  
- **SQL Server (MSSQL)** → Data storage, queries, and aggregations  
- **Power BI** → Data visualization and dashboard creation  

## 📂 Dataset  
The dataset includes transaction-level details:  
- `InvoiceNo` – Unique transaction identifier  
- `StockCode` – Product identifier  
- `Description` – Product name  
- `Quantity` – Number of items purchased  
- `InvoiceDate` & `InvoiceTime` – Transaction timestamps  
- `UnitPrice` – Price per unit  
- `CustomerID` – Unique customer identifier  
- `Country` – Customer’s location  

## 📊 Key Analyses  

### 🔹 **1. Customer Segmentation (RFM Analysis)**  
- Categorized customers into **Best, Loyal, At Risk, and New Customers**  
- Used **NTILE(4) in SQL** to segment customers based on spending behavior  

```sql
CREATE VIEW vw_RFM_analysis AS
SELECT 
    CustomerID,
    DATEDIFF(DAY, MAX(InvoiceDate), '2011-12-31') AS Recency,
    COUNT(DISTINCT InvoiceNo) AS Frequency,
    SUM(Quantity * UnitPrice) AS Monetary
FROM vw_orders_with_type
GROUP BY CustomerID;
```
### 🔹 **2. Revenue Trends (Monthly & Cumulative)**
- Tracked total revenue growth over time
- Identified seasonal fluctuations and peak months
```sql
CREATE VIEW vw_monthly_revenue AS
SELECT 
    FORMAT(CAST(InvoiceDate AS DATE), 'yyyy-MM') AS YearMonth,
    SUM(Quantity * UnitPrice) AS TotalRevenue
FROM vw_orders_with_type
GROUP BY FORMAT(CAST(InvoiceDate AS DATE), 'yyyy-MM')
ORDER BY YearMonth;
```
### 🔹 3. Peak Purchase Hours**
- Found the most active shopping periods
- Helped optimize inventory and marketing strategies
```sql
CREATE VIEW vw_peak_hours AS
SELECT 
    DATEPART(HOUR, InvoiceTime) AS PurchaseHour,
    COUNT(*) AS TotalOrders
FROM vw_orders_with_type
GROUP BY DATEPART(HOUR, InvoiceTime)
ORDER BY PurchaseHour;
```
### 🔹 4. Top 10 Best-Selling Products**
- Identified which products generated the most revenue
```sql
CREATE VIEW vw_Top10_items_sold AS
SELECT TOP 10
    Description,
    SUM(Quantity) AS QTY_sold
FROM vw_orders_with_type
GROUP BY Description
ORDER BY QTY_sold DESC;
```
## 📊 Power BI Dashboard
The final dashboard visualizes:

- Customer segmentation (RFM Analysis)
- Monthly revenue trends
- Retention rate
- Best-selling products
- Peak purchase hours
- Revenue by country
![image](https://github.com/user-attachments/assets/a46029e1-5da2-46b2-b91e-6a8cf34d0526)

## 📌 Key Takeaways
✅ High-Value Customers Drive Revenue → A small percentage of customers contribute to the majority of sales
✅ Peak Sales Time: 12 PM - 3 PM → Opportunity for targeted promotions
✅ The Most Popular Product: World War 2 Gliders

## 🔜 Next Steps
- Enhance Customer Segmentation → Add demographic insights
- Monitor Performance KPIs → Track key metrics over time
- Integrate More Data Sources → Add marketing & customer feedback data
## 📥 Repository Contents
- SQL Scripts/ → All SQL queries used for analysis
- Power BI Dashboard/ → .pbix file with interactive visualizations
- Dataset/ → Original dataset
- Screenshots/ → Power BI dashboard previews
## 💡 How to Use This Project
- Load the dataset into SQL Server
- Run the SQL scripts to create views and analyze data
- Open Power BI, connect it to SQL, and use the dashboard for insights
