# Walmart-Sales-Data

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
   - What is the relationship between high fuel prices and lower sales?
     
     How to Interpret (Quick Insight)
•	Positive value → factor increases sales
•	Negative value → factor decreases sales
•	Closer to ±1 → stronger relationship
•	Close to 0 → weak/no impact

   - How does the unemployment rate affect weekly sales?
How to Interpret (Quick Insight)
•	Positive value → factor increases sales
•	Negative value → factor decreases sales
•	Closer to ±1 → stronger relationship
•	Close to 0 → weak/no impact

     
# 7. Findings or Results
# 8. Recommendations
# 9. Limitations

