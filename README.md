# **Car sales**
## Project overview
Our company is a car dealership that sells various car models. To effectively track and analyse our sales performance, we need a comprehensive Car Sales Dashbord in Power BI.
## Data source
[car sales data](https://drive.google.com/drive/folders/1h6RQTGkoMOqovVUh9qf_nX5bI1_Meg8w)
## Tools
Power BI
## Data preparation 
1. Data loading.
2. Promoted headers.
3. Replaced value
4. Changed type.
## Exploratory data analysis
### Problem Statement 1: KPI’s Requirement
The dashboard should provide real-time insights into key performance indicators (KPIs) related to our sales data. This will enable us to make informed decisions, monitor our progress, and identify trends and opportunities for growth.
1.	Sales Overview:
-	Year-to-Date (YTD) Total Sales
-	Month-to-Date (MTD) Total Sales
-	Year-over-Year (YOY) Growth in Total Sales
-	Difference between YTD Sales and Previous Year-to-Date (PTYD) Sales
2.	Average Price Analysis:
-	YTD Average Price
-	MTD Average Price
-	YOY Growth in Average Price
-	Difference between YTD Average Price and PTYD Average Price
3.	Cars Sold Metrics:
-	YTD Cars Sold
-	MTD Cars Sold
-	YOY Growth in Cars Sold
-	Difference between YTD Cars Sold and PTYD Cars Sold
### Problem Statement 2: Charts Requirement

1.	YTD Sales Weekly Trend: Display a line chart illustrating the weekly trend of YTD sales. The X-axis should represent weeks, and the Y-axis should show the total sales amount.
2.	YTD Total Sales by Body Style: Visualize the distribution of YTD total sales across different car body styles using a Donut chart.
3.	YTD Total Sales by Color: Present the contribution of various car colors to the YTD total sales through a Donut chart.
4.	YTD Cars Sold by Dealer Region: Showcase the YTD sales data based on different dealer regions using a map chart to visualize the sales distribution geographically.
5.	Company-Wise Sales Trend in Grid Form: Provide a tabular grid that displays the sales trend for each company. The grid should showcase the company name along with their YTD sales figures.
6.	Details Grid Showing All Car Sales Information: Create a detailed grid that presents all relevant information for each car sale, including car model, body style, colour, sales amount, dealer region, date, etc
## Data analysis
- Add calendar table:
> `Calender Table = CALENDAR(MIN(car_data[Date]), MAX(car_data[Date]))`
- Add new columns
> `Year = YEAR('Calender Table'[Date])`
> 
> `Month = FORMAT('Calender Table'[Date], "MMMM")`
> 
> `Week = WEEKNUM('Calender Table'[Date])`
- DAX functions for sales overview
 > `YTD Total sales = TOTALYTD(SUM(car_data[Price ($)]), 'Calender Table'[Date])`
 >
 > `PYTD Total Sales = CALCULATE(SUM(car_data[Price ($)]), SAMEPERIODLASTYEAR('Calender Table'[Date]))`
 >
 > `Sales Difference = [YTD Total sales]-[PYTD Total Sales]`
 >
 > `Sales Differece Color = IF([Sales Difference]>0, "Green", "Red")`
 >
 > `YOY Sales Growth = [Sales Difference]/[PYTD Total Sales]`
 >
 > `MTD Total Sales = TOTALMTD(SUM(car_data[Price ($)]), 'Calender Table'[Date])`
 >
 > `MTD KPI = CONCATENATE("MTD Total Sales :", FORMAT([MTD Total Sales]/1000000, "$0.00M"))`

 - DAX functions for average price
  >
  > `Avg Price = SUM(car_data[Price ($)]) / COUNT(car_data[Car_id])`
  > 
  > `YTD Avg Price = TOTALYTD([Avg Price], 'Calender Table'[Date])`
  >
  > `PYTD Avg Price = CALCULATE([Avg Price], SAMEPERIODLASTYEAR('Calender Table'[Date]))`
  >
  > `Avg Price Diff = [YTD Avg Price] - [PYTD Avg Price]`
  >
  > `Avg Price Color = IF([Avg Price Diff]>0, "Green", "Red")`
  >
  > `YOY Avg Price Growth = [Avg Price Diff] / [PYTD Avg Price]`
  >
  > `MTD Avg Price = TOTALMTD([Avg Price], 'Calender Table'[Date])`
  >
  > `MTD Avg Price KPI = CONCATENATE("MTD Avg Price :", FORMAT([MTD Avg Price] / 1000, "$0.00K"))`
  >
 - DAX functions for cars sold metrics
 > `YTD Cars Sold = TOTALYTD(COUNT(car_data[Car_id]), 'Calender Table'[Date])`
 >
 > `PYTD Cars Sold = CALCULATE(COUNT(car_data[Car_id]), SAMEPERIODLASTYEAR('Calender Table'[Date]))`
 >
 > `Cars Sold Diff = [YTD Cars Sold]- [PYTD Cars Sold]`
 >
 > `Cars Sold Color = IF(car_data[Cars Sold Diff]>0, "Green", "Red")`
 >
 > `YOY Car Sold Growth = [Cars Sold Diff] / [PYTD Cars Sold]`
 >
 > `MTD Cars Sold = TOTALMTD(COUNT(car_data[Car_id]), 'Calender Table'[Date])`
 >
 > `MTD Cars Sold KPI = CONCATENATE("MTD Cars Sold :", FORMAT([MTD Cars Sold] / 1000, "$0.00K"))`
 - DAX function for max point 
 > `Max Point  = IF(MAXX(ALLSELECTED(‘Calendar Table’[Week]), [Total Sales]) = [Total Sales], MAXX(ALLSELECTED(‘Calendar Table’[Week]), [Total Sales]), BLANK())
