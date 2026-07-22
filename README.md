# cars-powerBi

**Jcars Project Objective**

The objective of the Jcars Project was to analyze vehicle sales data and develop an interactive Power Bi Dashboard that enables management to monitor sales performance, profitability, customer behaviour and branch performance.

The dashboard provides insights that support data-driven decision mking to improve revenue, increase profits and optimize business operations.

**How I imported the Data into Aiven**

1. I created a PostgreSQL database instance on Aiven
2. Obtained the database connection details (host, port, database name, username and password)
3. Imported the cleaned Jcars dataset into the PostgreSQL tables using SQL import commands.
4. Cross-checked that all records were successfully loaded.

# **How I Connected Power Bi to the Database**
1. Opened Power Bi Desktop
2. Selected **Get Data** - **PostgreSQL Database**
3. Entered the Aiven PostgreSQL server name and database name.
4. Provided the database credentials.
5. Loaded the data into power Bi.

# **The Measures and Calculations I Created**

Some of the DAX Measures used include:
1. **Total Sales Revenue** = sum('power bi jcars'[Revenue Recorded.1])
2. **Total Cars Sold** = SUM('power bi jcars'[Units Sold])
3. **Number of Orders**= DISTINCTCOUNT('power bi jcars'[Order ID])
4. **Gross Profit** = SUMX('power bi jcars','power bi jcars'[Revenue Recorded.1]+'power bi jcars'[Delivery Fee.1]- (DIVIDE('power bi jcars'[Revenue Recorded.1],'power bi jcars'[Unit Selling Price], 0)*'power bi jcars'[Unit Costt])-'power bi jcars'[Logistics Cost.1])
5. **Gross Profit Margin** = DIVIDE([Gross Profit],SUM('power bi jcars'[Revenue Recorded.1]),0)
6. **Top Delivery Status** = VAR TopStatus = TOPN(1,VALUES('power bi jcars'[Delivery Status]),[Number of Orders], DESC)VAR StatusName =MAXX(TopStatus,'power bi jcars'[Delivery Status])VAR OrderCount =MAXX(TopStatus,[Number of Orders]) RETURN StatusName & "  = " & FORMAT(OrderCount, "#,##0") & " Orders"
7. **Top Lead Source By Revenue** = VAR TopLead  = TOPN(1,VALUES('power bi jcars'[Lead Source]),[Total Sales Revenue],DESC) RETURN MAXX(TopLead,'power bi jcars'[Lead Source])& "_" & FORMAT (MAXX(TopLead,[Total Sales Revenue]), "#,##0")
8. **Top Payment Method by Revenue** = VAR TopPayment =TOPN(1,VALUES('power bi jcars'[Payment Method]),[Total Sales Revenue],DESC) RETURN MAXX(TopPayment,'power bi jcars'[Payment Method]) & "_" & FORMAT (MAXX(TopPayment,[Total Sales Revenue]), "#,##0")
9. **Top Customer Type Revenue** = VAR TopCustomer =TOPN(1,VALUES('power bi jcars'[Customer Type]),[Total Sales Revenue],DESC)RETURN MAXX(TopCustomer,'power bi jcars'[Customer Type]) & "  = " & FORMAT(MAXX(TopCustomer,[Total Sales Revenue]), "#,##0")
10. **Top Sold Car Model** = MAXX(TOPN(1,VALUES('power bi jcars'[Car Model]),[Total Cars Sold],DESC),'power bi jcars'[Car Model]
11. **Average Delivery Time(Days)** = AVERAGEX('power bi jcars',DATEDIFF('power bi jcars'[Order Date.1],'power bi jcars'[Delivery Date.1],DAY))
12. **Average Rating** = average('power bi jcars'[Customer Rating])
13. **Best Car Make and Model Profit Margin**= VAR TopCar = TOPN(1,SUMMARIZE('power bi jcars','power bi jcars'[Car Make],'power bi jcars'[Car Model],"Profit Margin",[Gross Profit Margin]),[Profit Margin],DESC)RETURN MAXX(TopCar, 'power bi jcars'[Car Make])& "  = " & MAXX(TopCar,'power bi jcars'[Car Model]) & " (" & FORMAT(MAXX(TopCar,[Gross Profit Margin]), "0.00%") & ")"
14. **Best Performing Vehicle Type** = VAR TopRevenue=MAXX(TOPN(1,VALUES('power bi jcars'[Vehicle Type]),[Total Sales Revenue],DESC),'power bi jcars'[Vehicle Type]) Var TopUnits=MAXX(TOPN(1,VALUES('power bi jcars'[Vehicle Type]),[Total Cars Sold],DESC),'power bi jcars'[Vehicle Type]) RETURN "Revenue: " & TopRevenue & " | Units Sold: " & TopUnits
15. **Most Returned or Cancelled Car** = VAR TopCar =TOPN(1,SUMMARIZE('power bi jcars','power bi jcars'[Car Model],"ReturnCancelCount",CALCULATE(COUNTROWS('power bi jcars'),'power bi jcars'[Delivery Status] IN {"Returned","Cancelled"})),[ReturnCancelCount],DESC) RETURN MAXX(TopCar,'power bi jcars'[Car Model]) & "  = " & FORMAT(MAXX(TopCar,[ReturnCancelCount]), "#,##0") & " returns/cancellations"
16. **Top Branch By Revenue** = MAXX(TOPN(1,VALUES('power bi jcars'[Branch]),[Total Sales Revenue],DESC),'power bi jcars'[Branch])

# **The Visuals Included in my Dashboard**


1. **KPI Cards**

The KPI cards provide a high-level overview of the business performance, allowing users to quickly assess the most important metrics without navigating through detailed reports.

Total Sales Revenue – Displays the total revenue generated from all vehicle sales, giving management an overall view of business performance.

Total Cars Sold – Shows the total number of vehicles sold during the selected period, which helps measure sales volume.
Number of Orders – Indicates the total number of customer orders processed, allowing comparison between orders placed and vehicles sold.

Gross Profit – Displays the total profit earned after deducting the cost of sales, helping evaluate the company's profitability.

Average Rating – Shows the average customer rating, providing insight into customer satisfaction and service quality.

2. **Line Chart (Monthly Sales Performance)**

The line chart visualizes Total Sales Revenue, Total Cars Sold, and Gross Profit by Month.

This visual helps users:

Identify monthly sales trends.

Compare how revenue, sales volume, and profit change over time.

Detect seasonal patterns or periods of high and low performance.

Evaluate whether increases in sales volume correspond to increases in profit and revenue.

The line chart makes it easier for management to monitor business growth and identify months that require further investigation.

3. **Bar Chart (Total Sales Revenue by Car Make)**
   
The bar chart compares the revenue generated by each car manufacturer.

This visual enables users to:

Identify the highest and lowest revenue-generating car makes.

Compare sales performance across different brands.

Support inventory planning by highlighting the most profitable brands.

Inform marketing strategies by focusing on high-performing manufacturers.

In this dashboard, the chart clearly shows that Toyota generated the highest sales revenue.

4. **Slicers (Interactive Filters)**

The slicers allow users to interact with the dashboard by filtering all visuals simultaneously.

Month – Filters the dashboard to analyze performance for a specific month or time period.

Region – Enables comparison of sales performance across different geographical regions.

Car Make – Filters the report to focus on a specific vehicle manufacturer, such as Toyota or Nissan.

Payment Status – Allows users to analyze sales based on payment completion status.

Vehicle Type – Filters the dashboard by categories such as SUV, Sedan, Pickup, or Hatchback, enabling users to compare performance across vehicle types.

These slicers make the dashboard dynamic, allowing users to perform self-service analysis without modifying the underlying data.

Why These Visuals:

The visuals were selected because they present information in a clear and intuitive manner:

KPI cards summarize key business metrics at a glance.

Line charts are ideal for showing trends over time.

Bar charts make it easy to compare categories such as car makes.

Slicers provide interactivity, allowing users to customize the dashboard based on their analysis needs.

# **Some of my Key Insights Include:**

From the dashboard, I discovered that:

A total of **258 cars** were sold.

The company generated approximately **KSH. 1 billion** in sales revenue.

Gross profit was **KSH 334.93 million,** resulting in a 24.83% gross profit margin.

**SUVs**generated the highest sales revenue, making them the best-performing vehicle category.

**Toyota** was the highest revenue generating car make.

**M-PESA**was the preferred payment method by revenue.

There were **255** Customers.

**Rift-Valley** was the top-performing region, while **Kiambu County** generated the highest county sales.

**Instagram** generated the highest revenue among all lead sources, showing the effectiveness of digital marketing.

Most orders had a **delivered status**, indicating efficient order fulfillment.

# **Recommendations**
1. Increase inventory and marketing for SUVs, especially Toyota models such as Harrier, since they drive the most revenue.
2. Invest more in instagram marketing because it generates the highest revenue from customerleads.
3. Focus on expanding sales efforts in Rift Valley and Kiambu while developing strategies to improve underperforming regions.
4. Strengthen relationships with Dealers through loyalty programs and exclusive offers, as they generate the highest revenue.
5. Maintain the high delivery Success rate by continuously monitoringlogistics performance.
6. Continue promoting M-Pesa as a payment option while ensuring other payment methods remain reliable.
