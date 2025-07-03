# KMS Business Intelligence Project

This project was developed as the final capstone of my Data Analysis training at **DSA Incubator Hub**.

Kultra Mega Stores (KMS), a leading office supplies and furniture retailer in Lagos, Nigeria, required actionable insights to support its Abuja division. Using sales data from 2009 to 2012, I applied SQL and data storytelling techniques to uncover trends, customer behavior, product performance, and shipping cost efficiency.

The goal was to assist management in making data-driven decisions by:
- Identifying high- and low-performing customers
- Analyzing product category profitability
- Evaluating regional sales distribution
- Matching shipping costs to order priorities
- Detecting patterns in product returns

This project demonstrates my ability to:
- Clean and transform raw datasets
- Perform SQL-based business analysis
- Translate data into insights for strategic action

📊 Tools used: **SQL Server**, **Excel**, **Power BI**  

📁 Files: `KMS Sql Case Study.csv`, `Order_Status.csv`
<img width="960" alt="KMS Sql Case Study" src="https://github.com/user-attachments/assets/cd4d66b5-e889-4639-a446-6431f3b5ff9d" />

<img width="960" alt="Order_Status" src="https://github.com/user-attachments/assets/adce9ef4-79d1-4d46-baec-177000d33f31" />



I'm proud to share this as a reflection of my growth and skill set developed through the DSA program.
*******************************************************************************************************************************************************
###  Task 1: Which product category had the highest sales?
```sql
SELECT 
    Product_Category,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Product_Category
ORDER BY [Total Sales] DESC;
```
<img width="960" alt="KMS Task 1" src="https://github.com/user-attachments/assets/e735ef3f-ba52-4914-8635-de1c1e367605" />


#### Objective:
To identify which product category generated the highest total sales revenue between 2009 and 2012.

#### Approach:
Using SQL aggregation, sales were grouped by Product_Category and ordered by total sales in descending order.

#### Findings:
Technology had the highest sales, totaling approximately $59.8 million.
It was followed by Furniture with about $51.8 million, and
Office Supplies with approximately $37.5 million.

#### *Insight:
Technology is KMS’s top-performing category by revenue and represents a key area for continued investment, promotions, and inventory prioritization.
*******************************************************************************************************************************************************

### Task 2: What are the Top 3 and Bottom 3 Regions in Terms of Sales?
```sql
SELECT TOP 3 
    Region,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Region
ORDER BY [Total Sales] DESC;
```
```sql
SELECT TOP 3 
    Region,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Region
ORDER BY [Total Sales] ASC;
```
<img width="960" alt="KMS Task 2" src="https://github.com/user-attachments/assets/c8086c77-46fe-4874-919a-1d01374bba02" />


#### Objective:
To identify the highest and lowest performing regions based on total sales volume.

#### Approach:
Two SQL queries were written using GROUP BY Region, one ordered in descending order to get the top 3 regions, and the other in ascending order for the bottom 3 regions.

#### Findings:

##### - Top 3 Regions:
1. West – $35.96M

2. Ontario – $30.63M

3. Prairie – $28.37M

##### - Bottom 3 Regions:
1. Nunavut – $1.16M

2. Northwest Territories – $800.8K

3. Yukon – $975.9K

#### Insight:
The West region led in sales, while remote regions like Nunavut and Yukon underperformed. This may be due to limited market access or demand, highlighting an opportunity for regional marketing or logistics strategy review.

*******************************************************************************************************************************************************

### Task 3: What Were the Total Sales of Appliances in Ontario?
```sql
SELECT 
    SUM(Sales) AS [Total Appliance Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE 
    Product_sub_Category = 'Appliances'
    AND Province = 'Ontario';
```
<img width="960" alt="KMS Task 3" src="https://github.com/user-attachments/assets/6426d5ce-3d23-46b8-b4c5-ab9b1d44cf1c" />


#### Objective:
To determine the total revenue generated from appliance sales specifically in the Ontario province.

#### Approach:
A SQL query filtered the dataset by:

Product_sub_Category = 'Appliances'

Province = 'Ontario'

Then, total sales were aggregated using SUM(Sales).

#### Finding: 
The total sales of appliances in Ontario amounted to $202,346.84

#### Insight:
Appliance sales in Ontario contributed a modest portion of overall revenue. KMS may explore boosting this category’s performance in the province through localized marketing, discounts, or product bundling.

*******************************************************************************************************************************************************

### Task 4: Advise KMS on What to Do to Increase Revenue from the Bottom 10 Customers
#### Objective:
To analyze the bottom 10 customers by total sales and provide actionable recommendations for improving revenue from them.

#### Key Insights from the Analysis:
##### Low Revenue & Profitability
```sql
SELECT TOP 10 
    Customer_Name,
    SUM(Sales) AS [Total Sales],
    SUM(Profit) AS [Total Profit],
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY [Total Sales] ASC;
```
<img width="960" alt="KMS Task 4 1" src="https://github.com/user-attachments/assets/2f325e23-d031-4dcc-8f29-0f80e1bb004c" />

The 10 lowest-paying customers each spent less than $500,000.

Some even generated negative profits, indicating unprofitable transactions.

##### Customer Segments
```sql
SELECT 
    Customer_Segment, 
    COUNT(DISTINCT Customer_Name) AS [Total Customers]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Segment
ORDER BY [Total Customers] ASC;
```
<img width="960" alt="KMS Task 4 2" src="https://github.com/user-attachments/assets/16b02d90-361e-4a82-a078-43137ba3cea9" />


These customers are spread across various segments, with a notable presence in Home Office and Consumer groups — the largest segments overall.

##### The products they buy
```sql
SELECT Customer_Name, Product_Category, SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Name IN (
    SELECT TOP 10 Customer_Name
    FROM [dbo].[KMS Sql Case Study]
    GROUP BY Customer_Name
    ORDER BY SUM(Sales) ASC  
)
GROUP BY Customer_Name, Product_Category
ORDER BY Customer_Name;
```
<img width="960" alt="KMS Task 4 3" src="https://github.com/user-attachments/assets/e50cb9db-9269-4f41-a3a7-b22c7f238d8a" />


1. Most low-spending customers primarily purchase Office Supplies, which tend to have lower margins.

2. A few (like Eric Murdock and Mark Hamilton) also bought from the Technology or Furniture categories, but in small quantities.

##### Shipping Modes & Regions
```sql
SELECT Customer_Name, Region, Ship_Mode, COUNT(*) AS OrderCount
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Name IN (
    SELECT TOP 10 Customer_Name
    FROM [dbo].[KMS Sql Case Study]
    GROUP BY Customer_Name
    ORDER BY SUM(Sales) ASC 
)
GROUP BY Customer_Name, Region, Ship_Mode;
```
<img width="960" alt="KMS Task 4 4" src="https://github.com/user-attachments/assets/0806550d-4377-4f0a-8bf2-934cbbc977e1" />


1. Regular Air was the most used shipping method.

2. Some customers (e.g., Christine Kargatis, Eric Murdock) used Express Air for low-value purchases — an inefficient shipping cost-to-value ratio.

3. Customers are scattered across various regions including West, Yukon, Ontario, Quebec, and Prarie.

#### Recommendations:
1. Implement targeted promotions based on their purchase history.

2. Offer personalized product bundles to encourage larger order sizes.

3. Introduce loyalty incentives for repeat orders.

4. Use cost-effective shipping incentives to improve margins.
These steps can help re-engage these low-performing customers and convert them into higher-value clients
*******************************************************************************************************************************************************
### Task 5: Which Shipping Method Incurred the Most Shipping Cost?
```sql
SELECT 
    Ship_Mode,
    SUM(Shipping_Cost) AS [Total Shipping Cost]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Ship_Mode
ORDER BY [Total Shipping Cost] DESC;
```
<img width="960" alt="KMS Task 5" src="https://github.com/user-attachments/assets/410dc619-0c90-4843-bfbd-1c13cf355274" />


#### Objective:
To identify the shipping method that resulted in the highest total shipping expenditure for KMS.

#### Approach:
A SQL aggregation was performed to sum Shipping_Cost for each Ship_Mode, ordered in descending order to highlight the most expensive method.

#### Findings:
Shipping Method	Total Shipping Cost
. Delivery Truck	$51,971.94
. Regular Air	$48,008.19
. Express Air	$7,850.91

#### Insight:
1. Delivery Truck was the most cost-intensive shipping method, accounting for the highest total shipping spend.

2. Despite being the slowest, it likely handles bulk or long-distance shipments.

3. Express Air, the fastest option, incurred the least cost, suggesting it's used sparingly or for smaller, urgent orders.

#### Recommendation:
1. Evaluate if Delivery Truck usage aligns with order value and urgency.

2. Consider switching some eligible bulk deliveries to Regular Air (if feasible) to balance speed and cost.

3. Implement shipping guidelines to prevent high-cost methods being used inefficiently.
*******************************************************************************************************************************************************

### Task 6: Who Are the Most Valuable Customers and What Do They Typically Purchase?
#### Objective:
To identify KMS's top 10 most valuable customers by total sales and understand their purchasing behavior.
##### Approach:
A SQL query was used to calculate:

Total sales (SUM(Sales))

Total profit (SUM(Profit))

Number of orders (COUNT(*))
Results were ordered by Total Sales in descending order.

```sql
SELECT TOP 10 
    Customer_Name,
    SUM(Sales) AS [Total Sales],
    SUM(Profit) AS [Total Profit],
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
GROUP BY Customer_Name
ORDER BY [Total Sales] DESC;
```
<img width="960" alt="KMS Task 6" src="https://github.com/user-attachments/assets/0abf0709-5800-42e1-9348-b860944ec0c7" />

#### Top 3 Most Valuable Customers:

Customer Name |	Total Sales	| Total Profit	| Order Count

Emily Phan    |	$117,124.44 |	$34,005.44    |	10

Deborah Brumfi|	$97,433.13	|$31,121.22	    |20

Roy Skaria    |	$92,542.15	|$1,343.94      |	26

#### Insight:
1. These customers contributed significantly to total revenue, especially Emily Phan, with over $117K in sales and $34K in profit from just 10 orders.

2. Some top customers (e.g., Julia Barnett) had negative profits, indicating that high sales don’t always guarantee profitability.

3. Customers like Darren Budd placed many smaller orders, while others made fewer, higher-value purchases.

#### Recommendation:
1. Maintain strong relationships with high-profit customers through loyalty programs or early-access promotions.

2. Review pricing or discounting strategies for high-sales, low-profit customers (e.g., Roy Skaria, Julia Barnett).

3. Personalize product recommendations based on past purchases to maximize customer lifetime value.
************************************************************************************************************************************************
### Task 7: Which Small Business Customer Had the Highest Sales?
#### Objective:
To identify the top customer in the Small Business segment by total sales between 2009 and 2012.
```sql
SELECT TOP 1 
    Customer_Name,
    SUM(Sales) AS [Total Sales]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Segment = 'Small Business'
GROUP BY Customer_Name
ORDER BY [Total Sales] DESC;
```
<img width="960" alt="KMS Task 7" src="https://github.com/user-attachments/assets/47ccdc10-b4e9-4626-8f68-9d023eaad1f8" />

#### Approach:
A SQL query filtered for Customer_Segment = 'Small Business', grouped the data by Customer_Name, and ordered the results by total sales in descending order.

#### Finding:
The top-performing small business customer was Dennis Kane With a total sales value of $75,967.59

#### Insight:
Dennis Kane represents a high-value account within the Small Business segment.

This customer may be a good candidate for relationship management, exclusive offers, or upsell campaigns to further drive sales and loyalty.
********************************************************************************************************************************************************
### Task 8: Which Corporate Customer Placed the Most Number of Orders (2009–2012)?
#### Objective:

To identify the Corporate segment customer with the highest number of orders placed during the period from 2009 to 2012.
```sql
SELECT TOP 1 
    Customer_Name,
    COUNT(*) AS [Order Count]
FROM [dbo].[KMS Sql Case Study]
WHERE 
    Customer_Segment = 'Corporate' AND
    TRY_CAST(Order_Date AS DATE) BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Name
ORDER BY [Order Count] DESC;
```
<img width="960" alt="KMS Task 8" src="https://github.com/user-attachments/assets/7e2e937e-85d4-4f81-a940-6cf1dc823edd" />

#### Approach:
A SQL query filtered by Customer_Segment = 'Corporate' and restricted the Order_Date to the target period. The data was grouped by Customer_Name and sorted by Order Count in descending order.

#### Finding:
The corporate customer with the most orders was Adam Hart, With a total of 27 orders placed between 2009 and 2012.

#### Insight:
Adam Hart demonstrated consistent purchasing behavior, making him a key customer to retain.
KMS could consider offering tailored services or volume-based incentives to maximize value from loyal corporate buyers like him.
********************************************************************************************************************************************************
### Task 9: Which Consumer Customer Was the Most Profitable One?
#### Objective:
To identify the customer within the Consumer segment who generated the highest total profit for KMS.
```sql
SELECT TOP 1 
    Customer_Name,
    SUM(Profit) AS [Total Profit]
FROM [dbo].[KMS Sql Case Study]
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Name
ORDER BY [Total Profit] DESC;
```
<img width="960" alt="KMS Task 9" src="https://github.com/user-attachments/assets/74ee3201-4372-41f9-8a55-3e6fbe3011b6" />

#### Approach:
A SQL query filtered the dataset for Customer_Segment = 'Consumer', grouped it by Customer_Name, and ordered the results by Total Profit in descending order.

#### Finding:
The most profitable Consumer customer was Emily Phan,

Generating a total profit of $34,005.44

#### Insight:
Emily Phan is not only a top spender (as seen in Task 6) but also the most profitable consumer, indicating high-margin purchases.

KMS should prioritize customer retention, offer exclusive benefits, and potentially use her purchasing patterns to profile similar high-value consumers.
************************************************************************************************************************************************************
### Task 10: Which Customer Returned Items, and What Segment Do They Belong To?
#### Objective:
To identify customers who returned items and determine the customer segment they belong to.


Approach:

Performed a LEFT JOIN between the main order data table and the Order_Status table using Order_ID.

Filtered for records where the Status = 'Returned'.

Grouped the results by Customer_Name and Customer_Segment to count the number of returns per customer.
```sql
SELECT 
    [dbo].[KMS Sql Case Study].*,
    [dbo].[Order_Status].[Status]
FROM [dbo].[KMS Sql Case Study]
LEFT JOIN [dbo].[Order_Status]
    ON [dbo].[KMS Sql Case Study].[Order_ID] = [dbo].[Order_Status].[Order_ID];
```


#### Finding:
A total of numerous customers across all segments (Consumer, Corporate, Home Office, Small Business) had returned orders.
```sql
SELECT 
    Customer_Name,
    Customer_Segment,
    COUNT(*) AS [Count_of_segment_return]
FROM [dbo].[KMS Sql Case Study]
LEFT JOIN [dbo].[Order_Status]
    ON [dbo].[KMS Sql Case Study].[Order_ID] = [dbo].[Order_Status].[Order_ID]
WHERE [Order_Status].[Status] = 'Returned'
GROUP BY Customer_Name, Customer_Segment
ORDER BY Count_of_segment_return DESC;
```
<img width="960" alt="KMS Task 10" src="https://github.com/user-attachments/assets/4cf7f4f9-c016-4be7-a925-16ed1c82dd94" />

The customers with the highest number of returns include:

Darren Budd (Consumer) – 10 returns

Erin Creighton (Corporate) – 10 returns

Olvera Toch (Home Office) – 8 returns

And several others across different segments with 5–7 returns

#### Insight:
Returns are spread across all segments, indicating a broad pattern rather than an isolated issue.

KMS should analyze the underlying causes (e.g., product quality, delivery issues) and consider strategies like:

   . Post-purchase follow-up

   . Improved product descriptions

   . Enhanced quality checks

   . Better customer education or training (if applicable)
*******************************************************************************************************************************************
### Task 11: Was Shipping Cost Appropriately Aligned with Order Priority?
#### Objective:
To evaluate whether KMS appropriately spent on shipping methods based on order priority, considering:

Delivery Truck = Most economical but slowest

Express Air = Fastest but most expensive
```sql
SELECT 
    Order_Priority,
    Ship_Mode,
    COUNT(*) AS [Order Count],
    SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM [dbo].[KMS Sql Case Study]
GROUP BY Order_Priority, Ship_Mode
ORDER BY Order_Priority, Total_Shipping_Cost DESC;
```
```sql
SELECT 
    Ship_Mode,
    COUNT(*) AS Total_Order_Count,
    SUM(Shipping_Cost) AS Total_Shipping_Cost
FROM 
    [dbo].[KMS Sql Case Study]
GROUP BY 
    Ship_Mode
ORDER BY 
    Total_Shipping_Cost DESC;
```
<img width="960" alt="KMS Task 11" src="https://github.com/user-attachments/assets/1f4112f4-3298-489c-a44e-68aaf1d72a59" />


#### Approach:
Aggregated total order count and shipping cost by Order_Priority and Ship_Mode.

Observed how often each shipping method was used for:

Critical

High

Medium

Low


Also summarized overall usage and costs per shipping method.

#### Key Findings:
Delivery Truck	|1146	|51971.9397373199
Regular Air	    |6270	|48008.189807415
Express Air   	|983	|7850.90996193886

Regular Air was the most used across all priorities — not just high ones.

Express Air, the fastest option, was used across all priorities, including Low and Not Specified, which likely do not require urgency.

Delivery Truck (most economical) was underutilized for low-priority orders where speed is less critical.

#### Conclusion:
No, KMS did not fully align shipping cost strategy with order urgency:

There’s inefficient spending, as Express Air was used even for low or unspecified priority orders.

Meanwhile, the cost-effective Delivery Truck could have absorbed more of the low-priority volume.

#### Recommendation:
Implement a shipping policy matrix that aligns shipping methods with order priorities:

Use Express Air strictly for Critical/High priority.

Use Delivery Truck or Regular Air for Medium/Low orders.

Add automated flags during order entry to suggest the optimal shipping mode.
