# Walmart-Sales-Data

## Table of Contents
- [Project Overview](#project-overview)
- [Data Source/s](#data-source/s)
- [Tools & Skillset](#tools-&-skillset)
- [Data Inspection, Cleaning & Validation](#data-inspection,-cleaning-&-validation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings or Results](#findings-or-results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
  
# 1. Project Overview 

This project analyzes Walmart's weekly sales data to uncover trends, seasonal patterns, and the impact of economic and environmental factors on retail performance, with the goal of supporting data-driven decision-making to help improve sales performance across all Walmart stores.

The dataset contains weekly sales records across multiple stores, along with influencing variables such as:

- Economic indicators (CPI, Unemployment)
- Operational costs (Fuel Price)
- Environmental conditions (Temperature)
- Seasonal effects (Holiday Flag)
- Unemployment Rate
  
## The analysis involves:

- Inspecting, cleaning, validating, and preparing the dataset for data analysis.
- Exploring trends and patterns in weekly sales.
- Comparing performance across stores.
- Evaluating relationships between sales and external factors.
  
# 2. Data Source/s
The dataset for this project was downloaded from Kaggle, and the excel sheet have been added to this repository:
- Attached excel file named: Walmart_DataSet(2).xlsx.
  
The dataset feautures the follong 8 columns and 51 488 rows:
- Store_Number
- Date
- Weekly_Sales
- Holiday_Flag
- Temperature
- Fuel_Price
- CPI
- Unemployment
  
# 3. Tools & Skillset
- Microsoft Excel
- Power Query Editor
- Power Pivot - DAX Measures
- Pivot tables Analysis and Slicers
  
# 4. Data Inspection, Cleaning & Validation

The initial stage of the analysis included 3 phases, which is data inspection, data cleaning and data validation phases.

## In this phase of the analysis, I performed the following activities:

4.1. After dowloading the dataset from Kaggle and saving in on my computer, I then opened it in Excel.
4.2. I checked and verified the structure of the dataset, by counting the total number of rows and columns of the dataset:
   
  - Examples of the Excel functions applied during the data inspection phase:
``` Excel
=COUNT()
=COUNTA()
=COLUMNS()
```

4.3. Loading the dataset into power query editor.
   
4.4. Checking, changing and verifying the data types:
- This include changing the data type of the 'Date' column, the column included different date formats; Example;
  
- In power query editor, select the 'Add Column' tab and choose 'Custom Column' at the ribbon. Modify the column by adding the following:
  
``` m
Text.SplitAny([Date],"/-,")
```

Add another custom column and editing it as the following: 

  ``` m
[Custom]{2}&"/"&[Custom]{1}&"/"&[Custom]{0}
```
4.5. Identifying and removing empty or null rows.
   - Using the select and merging the columns, and filtering and deleting all the nulls:
     
  ``` m
#"Inserted Merged Column" = Table.AddColumn(Source, "Merged", each Text.Combine({Text.From([Store], "en-ZA"), [Date], Text.From([Weekly_Sales], "en-ZA"), Text.From([Holiday_Flag], "en-ZA"), Text.From([Temperature], "en-ZA"), Text.From([Fuel_Price], "en-ZA"), Text.From([CPI], "en-ZA"), Text.From([Unemployment], "en-ZA")}, ""), type text),

#"Filtered Rows" = Table.SelectRows(#"Inserted Merged Column", each [Merged] <> null and [Merged] <> ""),

#"Removed Columns" = Table.RemoveColumns(#"Filtered Rows",{"Merged"}),
```
4.6. Removing duplicated rows from the dataset.
4.7. Identifiying the outliers, by finding the following per column: 
   - Minimum values
   - Maximum values
   - Total average values
   - Median.

4.8. Removing or deleting extra spaces.
    - To remove extra spaces from the dataset, I applied the following steps in every column in table:

``` m
#"Inserted Trimmed Text1" = Table.AddColumn(#"Removed Columns3", "Trim", each Text.Trim(Text.From([CPI], "en-ZA")), type text),
#"Removed Columns4" = Table.RemoveColumns(#"Inserted Trimmed Text1",{"CPI"}),
#"Reordered Columns1" = Table.ReorderColumns(#"Removed Columns4",{"Store_Number", "Date", "Weekly_Sales", "Holiday_Flag", "Temperature", "Fuel_Price", "Trim", "Unemployment"}),
#"Renamed Columns2" = Table.RenameColumns(#"Reordered Columns1",{{"Trim", "CPI"}}),
#"Changed Type5" = Table.TransformColumnTypes(#"Renamed Columns2",{{"CPI", type number}})

```
4.9. Creating Custom Columns
- In order to simplify my analysis, I have created 5 custom columns and they are as following:

  4.9.1. "Year" Column - to understand trend analysis over time.

  ```m
  = Table.AddColumn(#"Changed Type5", "Year", each Date.Year([Date]))

  ```
  4.9.2. "Month" Column - to understand seasonal and monthly patterns.

  ```m
  = Table.AddColumn(#"Removed Columns5", "Month", each Date.MonthName([Date]))

  ```
  4.9.3. "Week_Number" Column - to understand the performance of sales in a short-term basis.

   ```m
  = Table.AddColumn(#"Added Custom4", "Week_Number", each Date.WeekOfYear([Date]))

  ```
   4.9.4. "Season" Column - to understand business cycle insights.
  
```m
  = Table.AddColumn(#"Added Custom5", "Season", each if Date.Month([Date]) >= 12 or Date.Month([Date]) <= 2 then "Summer"
else if Date.Month([Date]) >= 3 and Date.Month([Date]) <= 5 then "Autumn"
else if Date.Month([Date]) >= 6 and Date.Month([Date]) <= 8 then "Winter"
else "Spring")
  ```

   4.9.5. Derive the "Holiday_Type" Column from the "Holiday_Flag" Column- to explore behavioral and event-based analysis.

   ```m
  = Table.AddColumn(#"Removed Columns6", "Holiday_Type", each if [#"Holiday_Flag"] = 1 then "Holiday" else "Non-holiday")

  ```
# 5. Exploratory Data Analysis

I've included the EDA to explore sales data in order to answer or uncover the following key questions or insights:

5.1. Sales Performance Insights or questions:
   - What is the average weekly sales across all Walmart stores?
   - Which stores has the lowest sales performance?
   - Across all the stores, wich one generates the highest total weekly sales?
   - How do weekly sales change over time?
   - Are there specific weeks or seasons with higher sales compared to other seasons?
  
5.2. Holiday impact questions or insights:
   - Which time period between holiday weeks or normal weeks produces higher sales?
   - Which stores perfom the best during holiday periods?
   - What is the percentage increase or decrease in sales during holidays?
     
5.3. Store-Level questions or insights:
   - Which Walmart stores show the most consistent weekly sales?
   - How does sales performance vary across all stores?
   - which stores show unusually high/ low sales patterns (outliers)?
     
5.4. The influence of economic factors related questions:
   - Is there a relationship between CPI and Walmart's weekly sales?
   - What is the relationship between high fuel prices and lower sales?
   - How does the unemployment rate affect weekly sales?
     
5.5. Questions or inights related to the impact of weather on sales:
   - Are extreme(too hot/ too cold) temperatures associated with higher or lower sales?

5.6. Insights or questions based on correlation and relationships:
   - Which factors (temperature, CPI data, unemployment data, and fuel price) have the stronged relationship with sales data.
   - What is the relationship between fuel price and CPI data?
  
5.7. Questions related to time-based trends: 
   - Are there specific dates or periods where sales spiked significantly?

# 6. Data Analysis
## 6.1. Analysing Sales Performance
   ### - Finding the average weekly sales accross all Walmart stores. 
           ```m
             =AVERAGE('Table1_1'[Weekly_Sales])
             ```

  ### - Finding the store/s with the lowest and highets sales performance.
    
    - By creating a pivot table and then dragging the "Weekly_Sales" column to "Values" field, the "Store_Number" column to "Rows" field.
    - On the pivot table, on the "Sum_Of_The_Weekly_Sales" column, sort the column in an ascending order, to find the stores with the    lowest sales, and sort in a  descending order to find the store with the highest sales performance.

  ### - Identifying weekly sales trend over time.
  
    - By creating a pivot table, Using the "Date", or "Years" or "Months" columns as  "Rows" and add "Weekly_Sales" column to the  "Values" field.
    
  ### - Identifying weekly or seasonal sales patterns.
    
## 6.2. Holiday Impact Analysis

  ### - Determining which time period between holiday weeks or normal weeks produces higher sales.
    
    - By creating a pivot table and adding the "Holiday_Type" column to the "Rows" field, and "Weekly_Sales" to the "Values" field. 
    - On the "Pivot Table Fields" right pane, navigate to "Values" field and click on "Weekly_Sales" and select the "Value field settings" option, the select "Average" to calculate the average sales for both holiday and non-holiday season.

 ### -Calculating the percentage increase or decrease in sales during holidays.

   #### - Using Power Pivot to create a measure for the "Holiday_Average_Sales".
```m
   =CALCULATE(
    AVERAGE(Table1_1[Weekly_Sales]),
    Table1_1[Holiday_Type]= "Holiday"
)
```

   #### - Using Power Pivot to create a measure for the "Non-holiday_Average_Sales".

```m
=CALCULATE(
    AVERAGE(Table1_1[Weekly_Sales]),
    Table1_1[Holiday_Type] = "Non-holiday"
)
```
   #### - The finally creating a measure to calculate the change in percentage (%) - using the formula:

```m
=([Holiday_Sales_Average] - [Non-holiday_Sales_Average]) / [Non-holiday_Sales_Average]

```
 ### -Identify which Walmart stores show the most consistent weekly sales.
    - By creating a pivot table to calculate the average sales per store.
 ### - Determine how sales performance vary across all stores.
    - By creating a pivot table to calculate the average sales and the standard deviation of the weekly sales per store.
## 6.3. Identify Store-Level Insights

 ### -Identify which stores show unusually high/ low sales patterns (outliers).

  #### Average_Sales_Per_Store
```m
  =AVERAGE('Table1_1'[Weekly_Sales])
```

  #### Overall_Average_Sales
```m


  =CALCULATE(
    AVERAGE('Table1_1'[Weekly_Sales]),
    ALL('Table1_1'[Store_Number])
)
```

 #### Z_Score_Measure - To Identify Outliers
```m
  =DIVIDE(
    [Average_Sales_Per_Store] - [Overall_Average_Sales],
    [Standard_Diviation]
)
```
## 6.4. Identifying the influence of economic factors of weekly sales.

 ### - Finding the relationship between CPI and Walmart's weekly sales
 
 #### 1. CPI_VS_Sales:
```m
   Mean_CPI
   
   =AVERAGE('Table1_1'[CPI])
   
   STDEV_CPI
   =CALCULATE(
    STDEV.P('Table1_1'[CPI]),
    ALL('Table1_1'[Store_Number])
)

CPI_VS_Sales

=VAR MeanSales = [Mean_Sales]
VAR MeanCPI = AVERAGEX(ALL('Table1_1'), 'Table1_1'[CPI])

VAR Numerator =
    SUMX(
        ALL('Table1_1'),
        ('Table1_1'[Weekly_Sales] - MeanSales) * ('Table1_1'[CPI] - MeanCPI)
    )

VAR DenomSales =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Weekly_Sales] - MeanSales) ^ 2)
    )

VAR DenomCPI =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[CPI] - MeanCPI) ^ 2)
    )

RETURN
    DIVIDE(Numerator, DenomSales * DenomCPI, 0)
```

#### 2. Unemployment_Vs_Sales
```m

Mean_Unemployment
=AVERAGE('Table1_1'[Unemployment])


=VAR MeanSales = [Mean_Sales]
VAR MeanUnemp = AVERAGEX(ALL('Table1_1'), 'Table1_1'[Unemployment])

STDEV_Unemployment
=STDEV.S('Table1_1'[Unemployment])

VAR Numerator =
    SUMX(
        ALL('Table1_1'),
        ('Table1_1'[Weekly_Sales] - MeanSales) * ('Table1_1'[Unemployment] - MeanUnemp)
    )

VAR DenomSales =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Weekly_Sales] - MeanSales) ^ 2)
    )

VAR DenomUnemp =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Unemployment] - MeanUnemp) ^ 2)
    )

RETURN
    DIVIDE(Numerator, DenomSales * DenomUnemp, 0)
```

#### 3. FuelP_Vs_Sales

```m
Mean_Fuel
=AVERAGE('Table1_1'[Fuel_Price])

STDEV_Fuel
=STDEV.S('Table1_1'[Fuel_Price])

FuelP_Vs_Sales
=VAR MeanSales = [Mean_Sales]
VAR MeanFuel = AVERAGEX(ALL('Table1_1'), 'Table1_1'[Fuel_Price])

VAR Numerator =
    SUMX(
        ALL('Table1_1'),
        ('Table1_1'[Weekly_Sales] - MeanSales) * ('Table1_1'[Fuel_Price] - MeanFuel)
    )

VAR DenomSales =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Weekly_Sales] - MeanSales) ^ 2)
    )

VAR DenomFuel =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Fuel_Price] - MeanFuel) ^ 2)
    )

RETURN
    DIVIDE(Numerator, DenomSales * DenomFuel, 0)
```

#### 4. Temperature_Vs_Sales
```m
STDEV_Temperature
=STDEV.S('Table1_1'[Temperature])

Mean_Temperature
=AVERAGE('Table1_1'[Temperature])

Temperature_Vs_Sales
=VAR MeanSales = [Mean­_Sales]
VAR MeanTemp = AVERAGEX(ALL('Table1_1'), 'Table1_1'[Temperature])

VAR Numerator =
    SUMX(
        ALL('Table1_1'),
        ('Table1_1'[Weekly_Sales] - MeanSales) * ('Table1_1'[Temperature] - MeanTemp)
    )

VAR DenomSales =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Weekly_Sales] - MeanSales) ^ 2)
    )

VAR DenomTemp =
    SQRT(
        SUMX(ALL('Table1_1'), ('Table1_1'[Temperature] - MeanTemp) ^ 2)
    )

RETURN
    DIVIDE(Numerator, DenomSales * DenomTemp, 0)
```
     
# 7. Findings or Results

## 7.1. Sales Performance Findings

### 7.1.1. Average Weekly Sales:
- Using the DAX measure =AVERAGE('Table1_1'[Weekly_Sales])
- The average weekly sales across all Walmart stores in the dataset is approximately $1,046,965. This serves as a benchmark for identifying under and over performing stores.
  
### 7.1.2. Highest and Lowest Performing Stores:
- Pivot table analysis, sorting the Sum of Weekly Sales field in ascending and descending order, revealed the following performance extremes:
#### Top 5 High Performing Stores:
- First Highest : Store 20 @ R301 397 792 sum of total sales.
- Second Highest : Store 4 @ R299 543 953 sum of total sales.
- Third Highest : Store 14 @ R288 999 911 sum of total sales.
- Forth Highest : Store 13 @ R286 517 703 sum of total sales.
- Fifth Highest : Store 2 @ R 275 382 441 sum of total sales.

#### Top 5 Lowest Performing Stores: 
- First Lowest : Store 33 @ R371 602 22 sum of total sales.
- Second Lowest : Store 44 @ R432 930 88 sum of total sales.
- Third Lowest : Store 5 @ R454 756 89 sum of total sales.
- Forth Lowest : Store 36 @ R534 122 15 sum of total sales.
- Fifth Lowest : Store 38 @ R551 596 26 sum of total sales.

#### Conclusion:
- The significant gap between the highest and lowest performing stores suggests structural differences which could be driven by store size, location demographics, or local economic conditions — rather than operational inefficiency alone.

### 7.1.3. Weekly Sales Trend Over Time:
- Pivot table time-series analysis using Year and Month as row fields.
  
#### Conclusion:
- revealed a broadly upward sales trend across the dataset period (2010–2012), with notable volatility during Q4 each year driven by the holiday shopping season. Sales dip in January and February consistently, recovering through mid-year.
  
### 7.1.4. Seasonal Sales Patterns:

The analysis clearly revealed the following:

-	Qtr4 (October–December): Highest sales volumes driven by Thanksgiving, Black Friday, and Christmas shopping.
-	Qtr1 (January–February): Significant post-holiday dip across nearly all stores.
- Qtr2 & Qtr3: Moderate, stable sales with a minor uplift around July 4th and Labor Day.

## 7.2. Holiday Impact Analysis Results
### 7.2.1. Holiday vs. Non-Holiday Sales Findings:

- Using a pivot table with Holiday_Type in Rows and Average of Weekly_Sales in Values, the analysis confirmed that holiday weeks produce measurably higher sales than regular weeks:
- Holiday Average Sales : R1 122 888.
- Non-Holiday Average Sales: R1 041 256.

#### Conclusion: 

- The Power Pivot DAX measures for Holiday_Sales_Average, Non-holiday_Sales_Average, and the percentage change formula confirmed an approximate 7.2% uplift in average weekly sales during holiday-flagged weeks. This uplift, while consistent, is modest — suggesting that Walmart's everyday competitive pricing and broad product assortment maintain relatively stable foot traffic even outside holiday periods.

### 7.2.2. Best Performing Stores During Holiday Periods:

- Stores 20, 4, and 14 consistently recorded the highest absolute sales volumes during holiday weeks.
- Stores 10 and 13 showed disproportionately higher percentage uplift during holidays compared to their non-holiday baselines, suggesting strong community holiday shopping behaviour in those catchment areas.

## 7.3. Store-Level Insights Results: 
### 7.3.1. Sales Consistency 

#### Using a pivot table calculating both average sales and standard deviation per store was used to identify consistency.
- Stores with a lower coefficient of variation (standard deviation relative to mean) were deemed more consistent.
- Store 20 and Store 4 demonstrated both high average sales AND relatively low variability, making them the most reliably high-performing locations.

### 7.3.2. Sales Variability Across Stores

- The range between the highest and lowest weekly sales figures spans several orders of magnitude, pointing to highly heterogeneous store profiles. Smaller-format or rural stores (such as Store 33, 36, and 44) consistently underperform relative to the overall average.

### 7.3.3. Outlier Identification (Z-Score Analysis)

#### Three DAX measures were deployed:
- Average_Sales_Per_Store, Overall_Average_Sales, and Z_Score_Measure.
- Using the DIVIDE and CALCULATE(... ALL()) pattern to identify statistical outliers across the store network:

#### Top 5 stores classified as Low Outlier stores (Where Z-score is < -1,50): 
- Store 33 : -1,4
- Store 44 : -1,3
- Store 5 : -1,3
- Store 36 : -1,2
- Store 38 : -1.17

#### Top 5 stores classified as High Outlier stores (Where Z-score is > 1,50):
- Store 20 : 1,88
- Store 4 : 1,86
- Store 14 : 1,73
- Store 13 : 1, 70
- Store 2 : 1,56

## 7.4. Economic Factor Influence Results:

### 7.4.1. CPI vs. Weekly Sales
- Calculated via the custom DAX measure CPI_VS_Sales using SUMX over ALL rows;
- Showed a weak negative correlation (approximately rate = -7%).

#### Conclusion:
- This suggests that as the general cost of living (as measured by CPI) increases, Walmart's sales volumes show a very slight decline, though the relationship is not strong enough to be considered a primary driver.

- A plausible interpretation is that during periods of high inflation (elevated CPI), consumers may reduce discretionary spending. However, Walmart's value positioning as a discount retailer may partially insulate it from CPI pressure, as consumers trading down from premium retailers may offset losses from budget-constrained shoppers reducing basket sizes.

### 7.4.2. Unemployment Rate vs. Weekly Sales

- Calculated by creating an Unemployment_Vs_Sales DAX measure revealed a moderate negative correlation (approximately rate = -11%).
  
#### Conclusion:
- Higher unemployment rates are associated with lower weekly sales. This is the strongest economic relationship identified across the four tested variables, consistent with the intuition that unemployment directly reduces disposable income and consumer spending power.

### 7.4.3. Fuel Price vs. Weekly Sales

- The FuelP_Vs_Sales measure returned a near-zero or very weakly negative correlation (approximately rate = -2%).
- Fuel price appears to have minimal direct correlation with Walmart weekly sales in this dataset.
- This may reflect Walmart's geographic positioning (often accessible via short local trips) and the non-discretionary nature of much of its product assortment.

# 8. Recommendations
## 8.1.  Leverage High Performing Stores as Operational Benchmarks

#### Store 20 and 4 and the other 3 stores that were part of the top 5 high performing stores. A structured store benchmarking programme should be established to:

- Document and replicate the operational practices, staffing models, and inventory strategies of top-performing stores.
- Pilot best practices from Store 20 in mid-tier stores before rolling out network-wide.

## 8.2. Target Intervention for Low-Performing Stores

#### Stores flagged as low outliers (Z-score < -1.50), particularly Store 33, require immediate investigation. Recommended actions include:

- Conduct a root-cause analysis examining local demographics, store format, competitive landscape, and operational efficiency.
- Consider format restructuring (e.g., Neighborhood Market conversion) if the full-size format is unsuitable for the catchment area.
- Introduce localised promotional strategies tailored to community-specific demand patterns.

  ## 8.3. Place More Effort in Holiday Season Preparations
  
#### The 7.2% average sales uplift during holiday weeks represents a significant but potentially undercaptured opportunity. To maximise revenue during peak periods:

- Begin inventory build-up and staffing increases at least 4–6 weeks before major holidays, particularly Thanksgiving and Christmas.
- Implement targeted marketing campaigns for the pre-Christmas period (mid-November to December 24) across all stores.

## 8.4. Address the Post-Holiday Demand Trough

#### The consistent January sales dip represents a predictable challenge. Proactive strategies to mitigate this include:

- January sales events and clearance promotions to drive traffic after the holiday period.
- New Year / health-oriented product category promotions (fitness, wellness, fresh food) aligned with post-holiday consumer behaviour.
- Loyalty programme incentives activated in January to sustain engagement through the low-traffic period.

## 8.5. Monitor Unemployment as a Leading Sales Indicator

#### Given that unemployment shows the strongest correlation with weekly sales, Walmart's retail planning teams should:

- Integrate regional unemployment rate data into store-level sales forecasting models.
- Identify stores in high-unemployment catchment areas and implement value-focused marketing (price-match guarantees, bulk savings, private label promotion) to retain economically stressed shoppers.

# 9. Limitations

### While the findings of the analysis display a robust directional insights, several limitations should be considered when interpreting findings and making decisions based on this analysis:

- The Holiday_Type binary flag (Holiday / Non-Holiday) does not distinguish between different types of holidays (e.g., Thanksgiving vs. Christmas vs. Labor Day), which likely have very different magnitudes of sales impact. A more granular holiday classification would improve the precision of holiday impact analysis and recommendation targeting.
- The weekly aggregation of sales data may obscure intra-week patterns, such as weekend shopping peaks or weekday promotional performance. Daily-level data would enable more precise operational recommendations around staffing, inventory replenishment, and promotional scheduling.
- The analysis is entirely internal to Walmart's store-level data. Without competitor sales, pricing, and promotional data, it is impossible to determine whether observed sales trends reflect Walmart-specific dynamics or broader market movements. Integrating external competitive intelligence would substantially improve the explanatory power of the analysis.
