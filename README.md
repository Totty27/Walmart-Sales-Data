# Walmart-Sales-Data

# 1. Project Overview 

This project analyzes Walmart's weekly sales data to uncover trends, seasonal patterns, and the impact of economic and environmental factors on retail performance, with the goal of supporting data-driven decision-making.

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
- Store
- Date
- Weekly Sales
- Holiday Flag
- Temperature
- Fuel Price
- CPI
- Unemployment
  
# 3. Tools
- Microsoft Excel
- Power Query Editor
  
# 4. Data Inspection, Cleaning & Validation

The initial stage of the analysis included 3 phases, which is the data inspection, data cleaning and data validation phases.

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
- This included changing the data type of the 'Date' column, the column included different date formats; Example;
  
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

   4.9.5. "Holiday_Type" Column - to explore behavioral and event-based analysis.

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
# 7. Findings or Results
# 8. Recommendations
# 9. Limitations

