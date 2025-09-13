# 📦 E-Commerce Sales Dashboard  

## 📌 Project Overview
This project analyzes an **E-Commerce dataset** to uncover sales performance and profitability across categories, regions, and shipping methods.  
The insights are visualized through an **interactive Power BI dashboard**, helping businesses identify growth opportunities and optimize operations.  

---

## 📝 Executive Summary  
- 📉 Sales declined slightly (↓0.83% YoY), but **profitability improved** (↑4.5%), showing stronger cost efficiency.  
- 🏷️ **Office Supplies dominate** revenue, but over-reliance poses risk → need for diversification into Furniture & Tech.  
- 🌍 Sales concentrated in **West & East regions** + 4 major states, highlighting **geographic dependency**.  
- 🚚 **Late deliveries ($6.2M impact)** threaten customer satisfaction → logistics optimization is critical.  
- 📊 Dashboard enables leaders to track KPIs, uncover bottlenecks, and drive **growth + retention strategies**.  

---

## 📷 Dashboard Preview 
<img width="1203" height="734" alt="Ecommerce Sales Dashboard" src="https://github.com/user-attachments/assets/c7a09507-b16f-4d14-a9dd-be06b6d0d997" />

---

## 🗂️ Dataset Description  

1. The Fact Table contains **113,270 rows × 18 columns**, covering customer, product, sales, and shipping details. 
2. The State Description Table contains **52 rows × 4 columns**, covering States name, code, latitude & longitude.
3. The Calender Date Table contains **727 rows × 4 columns**, coverind dates, month, month as number & year.

### 1. Fact Table
| Column Name | Description |
|-------------|-------------|
| customer_id | Unique identifier for each customer |
| customer_first_name / customer_last_name | Customer details |
| category_name | Product category (Office Supplies, Furniture, Technology) |
| product_name | Specific product purchased |
| customer_segment | Segment (Consumer, Corporate, Home Office) |
| customer_city / state / region | Customer location |
| order_id | Unique identifier for each order |
| order_date | Date when the order was placed |
| ship_date | Shipping date |
| shipping_type | Standard, First Class, Second Class, Same Day |
| sales_per_order | Total sales amount for the order |
| profit_per_order | Profit generated from the order |
| order_quantity | Number of items purchased |


### 2.State Description
| Column Name | Description |
|-------------|-------------|
| State | Two-letter code for States |
| latitude | State latitude |
| Longnitude | State longitude |
| State Name | State Name |

## 3.Calender Date
| Column Name | Description |
|-------------|-------------|
| Date | All dates btw 2 dates |
| Year | Date's Year |
| Month | Month in "mmm" format  |
| Month Number | Month as Number |


---

## 🛠️ Tools Used  

- **Power Query** → Data Cleaning
- **Power BI** → Dashboard design & visualization
- **DAX** – for calculated measures    
- **Excel** → Data source  

---


## ❓ Problem Statements  

1. How do **YTD Sales, Profit, Quantity, and Profit Margin** compare against the previous year?  
2. Which **categories and products** drive the highest sales and profitability?  
3. What are the **regional sales trends** across the U.S.?  
4. Which **shipping methods** contribute most to revenue?  
5. How does **delivery status** (on-time vs. late) affect sales performance?  
6. What are the **top-performing states and products** by sales?  

---

## 📐 Key Measures (DAX)  

```DAX
-- Total Sales
Total Sales = SUM('Fact Table'[sales_per_order])

-- Total Profit
Total Profit = SUM('Fact Table'[profit_per_order])

-- YTD Sales
YTD Sales = TOTALYTD(SUM('Fact Table'[sales_per_order]),'Calender Date'[Date])

-- PY Sales
PY Sales = CALCULATE(SUM('Fact Table'[sales_per_order]),DATESYTD(SAMEPERIODLASTYEAR('Calender Date'[Date])))

-- Sales Indicator
Sales % with Indicator = 
VAR PctChange =
    DIVIDE ( [YTD Sales] - [PY Sales], [PY Sales], 0 )
VAR Arrow =
    IF (
        PctChange > 0,
        UNICHAR ( 9650 ),        -- ▲ Up
        IF (
            PctChange < 0,
            UNICHAR ( 9660 ),    -- ▼ Down
            UNICHAR ( 8212 )     -- ― No change
        )
    )
RETURN
FORMAT ( PctChange, "0.00%" ) & " " & Arrow

-- YTD Profit
YTD Profit = TOTALYTD(SUM('Fact Table'[profit_per_order]),'Calender Date'[Date])

-- PY Profit
PY Profit = CALCULATE(SUM('Fact Table'[profit_per_order]),DATESYTD(SAMEPERIODLASTYEAR('Calender Date'[Date])))

-- Profit Indicator
Profit % with Indicator = 
VAR PctChange =
    DIVIDE ( [YTD Profit] - [PY Profit], [PY Profit], 0 )
VAR Arrow =
    IF (
        PctChange > 0,
        UNICHAR ( 9650 ),        -- ▲ Up
        IF (
            PctChange < 0,
            UNICHAR ( 9660 ),    -- ▼ Down
            UNICHAR ( 8212 )     -- ― No change
        )
    )
RETURN
FORMAT ( PctChange, "0.00%" ) & " " & Arrow

-- YTD Quantity
YTD Quantity = TOTALYTD(SUM('Fact Table'[order_quantity]),'Calender Date'[Date])

-- PY Quantity 
PY Quantity = CALCULATE(SUM('Fact Table'[order_quantity]),DATESYTD(SAMEPERIODLASTYEAR('Calender Date'[Date])))

-- Quantity Indicator
Quantity % with Indicator = 
VAR PctChange =
    DIVIDE ( [YTD Quantity] - [PY Quantity], [PY Quantity], 0 )
VAR Arrow =
    IF (
        PctChange > 0,
        UNICHAR ( 9650 ),        -- ▲ Up
        IF (
            PctChange < 0,
            UNICHAR ( 9660 ),    -- ▼ Down
            UNICHAR ( 8212 )     -- ― No change
        )
    )
RETURN
FORMAT ( PctChange, "0.00%" ) & " " & Arrow

-- Profit Margin
Profit Margin = DIVIDE(SUM('Fact Table'[profit_per_order]),SUM('Fact Table'[sales_per_order]),0)

-- YTD Profit Margin  
YTD Profit Margin = TOTALYTD([Profit Margin],'Calender Date'[Date])

-- PY Profit Margin
PY Profit Margin = CALCULATE([Profit Margin],DATESYTD(SAMEPERIODLASTYEAR('Calender Date'[Date])))

-- Profit Indicator
Profit Margin % with Indicator = 
VAR PctChange =
    DIVIDE ( [YTD Profit Margin] - [PY Profit Margin], [PY Profit Margin], 0 )
VAR Arrow =
    IF (
        PctChange > 0,
        UNICHAR ( 9650 ),        -- ▲ Up
        IF (
            PctChange < 0,
            UNICHAR ( 9660 ),    -- ▼ Down
            UNICHAR ( 8212 )     -- ― No change
        )
    )
RETURN
FORMAT ( PctChange, "0.00%" ) & " " & Arrow

```


---

## 🔎 Detailed Analysis  

### 1. 📊 **Overall Sales & Profitability**  
- **YTD Sales:** $11.53M (↓0.83% vs PY)  
- **YTD Profit:** $1.34M (↑4.50% vs PY)  
- **YTD Quantity:** 107K units (↓7.29% vs PY)  
- **Profit Margin:** 11.58% (↑5.37% vs PY)  

#### 🔑 Key Insights:
- Even though sales volume and revenue slightly declined, profit and margins improved, suggesting better cost control, pricing efficiency, or reduced discounting.
- Profits grew significantly despite a sales drop → **better cost control and improved product mix**.  
- Indicates a shift toward profitability optimization rather than just revenue growth.

 #### 📌 Implication  
- The business is sacrificing sales volume for profitability, which may limit long-term market share if the trend continues.
- While profit margins are improving, declining sales volume could indicate weakening demand or reduced competitiveness.
- Sustained imbalance may lead to slower growth and potential customer attrition if not addressed.

---
### 2. 📅 **Sales Trend Analysis**  

- **Seasonality Peaks:** March, June, and November.  
- **YoY Comparison:**  
  - 2022 outperformed 2021 in H1 (Jan–Jun).  
  - 2022 underperformed in H2, especially in **Nov–Dec holiday season**.  
- **November Spike:** Remains the strongest month due to festive/holiday promotions.

#### 🔑 Key Insights:
- Sales show peaks around **March, June, and November**, suggesting demand cycles linked with promotions or seasonal buying.
- Strong H1 but weak H2 suggests **competition, supply chain bottlenecks, or weak promotions** at year-end.  
- Stable monthly revenue (~$0.8M–$1.1M) shows resilience, but **growth is inconsistent**.  
- Seasonality is predictable → ideal for **forecasting demand and optimizing inventory**.  

#### 📌 Implication  
- The business is highly seasonality-driven, with heavy reliance on promotional cycles (March, June, November).
- Weak performance in H2 (especially holidays) suggests lost opportunities in a period where competitors may be stronger.
- Revenue stability indicates a resilient customer base, but inconsistent growth highlights vulnerability to external factors (competition, supply chain, promotions).
- Predictable cycles create both an opportunity (better planning) and a risk (over-dependence on peaks). 

<img width="705" height="417" alt="Trend analysis" src="https://github.com/user-attachments/assets/a693ce16-60de-423e-99a6-9a71d26a368f" />

---

### 3. 🗂️ **Category Analysis**  
- **Office Supplies**: $6.92M sales (↓1.22% vs PY)  
- **Furniture**: $2.52M sales (↑0.73% vs PY)  
- **Technology**: $2.10M sales (↓1.37% vs PY)  

#### 🔑 Key Insights:
- **Office Supplies** is the largest contributor, but declining → risk of market saturation.  
- **Furniture** shows positive growth → expansion potential in corporate/home-office setups.  
- **Technology** declined despite high margins → needs innovation-driven marketing and bundles.  

#### 📌 Implication  
- Heavy reliance on Office Supplies makes revenue vulnerable to stagnation if the category continues to decline.
- Furniture growth indicates an emerging opportunity to tap into higher-value purchases (corporate and home office markets).
- The decline in Technology sales highlights a missed opportunity in a high-margin segment, possibly due to weak positioning or competition.
- Lack of balanced performance across categories risks long-term growth stability.

<img width="642" height="309" alt="Sales by Category" src="https://github.com/user-attachments/assets/f9a72132-57b8-4615-afd4-0a9362d3e5bf" />

---

### 4. 🏷️ **Top Products**  
- **Staple Envelope – $57K**  
- **Staples – $52K**  
- **Easy-Staple Paper – $47K**  
- Other contributors: Misc. Staples, **KI Adjustable Chairs**.   

#### 🔑 Key Insights:
- Top products are **low-cost, high-frequency consumables** → consistent revenue drivers.  
- Presence of **Furniture (KI Chairs)** in the top list shows potential in **higher-value categories**.  
- Over-dependence on **consumables** poses risk → margins are thinner and depend on volume.

#### 📌 Implication  
- Heavy reliance on consumables makes revenue vulnerable to volume fluctuations and competitive pricing.
- Inclusion of furniture (KI Chairs) among top products suggests an untapped growth potential in higher-value categories.
- Without diversification, profitability may stagnate despite stable sales.

<img width="706" height="463" alt="Top Products" src="https://github.com/user-attachments/assets/1f5669aa-751d-42ce-9867-a2f43571b891" />

---

### 3. 🌍 **Regional Trends**  
- **West region leads** with 32% of total sales.  
- **East** (28%) and **Central** (23%) follow.  
- **South** region lags with 16%.  

#### 🔑 Key Insights:
- **West & East** account for **~60% of sales**, making them the company’s backbone markets.  
- **Central** is stable but under-leveraged → could be improved with localized promotions and better supply chain efficiency.  
- **South** underperforms but has **large untapped potential**. Weak performance may be due to **logistics gaps, brand awareness issues, or competition**.  

#### 📌 Implication  
- The company’s revenue is heavily concentrated in the West and East regions, creating dependency risk if these markets stagnate.
- Central region shows steady sales but lacks strong momentum, signaling untapped demand or weaker engagement.
- The South region’s underperformance highlights significant growth potential, but also exposes structural challenges (logistics, competition, brand presence).
- Uneven regional distribution suggests that the business is not yet fully capitalizing on national market potential.

<img width="600" height="368" alt="Reginal Sales" src="https://github.com/user-attachments/assets/b425a92c-aaef-414f-aeb4-76e22ca76cf5" />

---
### 6. 🗺️ **State-wise Insights**  
- High sales concentration in **California, New York, and Texas**.  
- Potential to strengthen market in less-penetrated states.  

#### 🔑 Key Insights:  
- Heavy reliance on top 4 states → concentration risk.  
- **California & New York** alone contribute a significant share → critical for future strategies.  
- Smaller states and rural areas are **under-penetrated**, showing an opportunity for **targeted digital campaigns and affordable shipping**.

#### 📌 Implication  
- The business is heavily reliant on a few large states (California, New York, Texas), which creates a concentration risk if demand weakens in these markets.
- Dependence on dense urban markets indicates limited geographic diversification.
- Under-penetration in smaller states suggests lost revenue opportunities and slower national growth.
- Weak reach in rural/less-developed states reflects structural challenges like higher delivery costs and lower brand awareness. 

<img width="646" height="567" alt="Sales by States" src="https://github.com/user-attachments/assets/1f1e4bc7-5cdc-4c0f-9df5-9653af435b23" />

---

### 5. 🚚 **Shipping & Delivery**  
- **Standard Class** accounts for 61% of orders. 
- **Second Class** → 19%, **First Class** → 15%, **Same Day** → 5%. 
- Late deliveries ($6.24M sales) impact customer experience significantly.
- Advance Shipping: $2.71M, others ~$2M.  

#### 🔑 Key Insights:
- Heavy reliance on **Standard Class** shipping.
- Improving logistics could boost customer retention.
- **Late deliveries are significant** (over 50% of revenue) → potential customer dissatisfaction.
- Opportunity to promote premium shipping methods (First Class, Same Day) to reduce delays. 
- Expanding Same-Day delivery in urban hubs can boost loyalty and capture a competitive edge.

#### 📌 Implication  
- Heavy dependence on Standard Class (61% of orders) shows cost-sensitivity among customers but also creates a service quality risk if delays persist.
- A significant share of late deliveries ($6.24M sales) suggests that logistics inefficiencies are eroding customer experience, even in cost-effective segments.
- Premium shipping options (First Class, Same Day) are underutilized, limiting opportunities to build loyalty among high-value, time-sensitive customers.
- The shipping mix reflects a trade-off between affordability and reliability that may constrain long-term customer satisfaction and retention.


---


## 💡 Recommendations (Strategic Next Steps) 


1. **Stabilize Sales & Boost Demand** → Use targeted promotions during off-peak months, optimize pricing strategies, and launch loyalty programs to counter declining quantities.  

2. **Focus on Furniture Growth** → Expand corporate/B2B campaigns and cross-selling to build on the slight growth trend.  

3. **Revamp Technology Category** → Refresh the product mix, improve after-sales support, and offer competitive discounts to regain market share.  

4. **Diversify Product Portfolio** → Reduce reliance on top sellers by introducing premium versions, bundled offers, and marketing support for niche products.  

5. **Regional Expansion in South & Central** → Run localized campaigns, expand distributor networks, and strengthen weaker regions while protecting dominance in West & East.  

6. **State-Level Penetration** → Improve digital presence in smaller states and target rural areas with affordable product lines to reduce geographic concentration risk.  

7. **Shipping Mix Optimization** → Promote Same-Day/Next-Day options for premium customers while incentivizing advance shipping for better planning. 

8. **Improve Delivery Performance** → Reduce $6.24M worth of late deliveries by strengthening logistics and supplier management.  

 

---

## 📈 Business Impact  

- **Informed Decision-Making** – Dashboard enables managers to quickly identify top-performing regions, products, and customers.  
- **Revenue Growth** – Strategic recommendations can increase sales through category optimization and targeted campaigns.  
- **Customer Retention** – Insights into loyalty and shipping preferences help reduce churn and improve satisfaction.  
- **Operational Efficiency** – Identifies shipping delays and logistic bottlenecks for faster corrective action.  
- **Market Expansion** – Highlights underperforming regions, enabling leadership to allocate resources strategically.  

---
## 🚀 Future Scope / Next Steps  
- Integrate **machine learning models** for sales forecasting.  
- Apply **RFM segmentation** for better customer targeting.  
- Automate dashboard refresh via **Power BI Service**.  
- Add competitor/market benchmarking for strategic comparison. 

--- 

## 📂 Project Structure  

```plaintext
/data              → contains raw & cleaned datasets
/dashboard         → Power BI file (.pbix)
/images            → dashboard screenshots
README.md          → project documentation
```

## 🙌 Acknowledgements  
- Dataset: E-commerce (Superstore-style)  
- Tools: Power BI, DAX, Excel  
- Inspiration: Microsoft Power BI Community 
