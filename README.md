# Walmart-Sales-Data

# Project Overview 

This project analyzes Walmart's weekly sales data to uncover trends, seasonal patterns, and the impact of economic and environmental factors on retail performance, with the goal of supporting data-driven decision-making.

The dataset contains weekly sales records across multiple stores, along with influencing variables such as:

- Economic indicators (CPI, Unemployment)
- Operational costs (Fuel Price)
- Environmental conditions (Temperature)
- Seasonal effects (Holiday Flag)
- Unemployment Rate
  
## The analysis will involve:

- Inspecting, cleaning, validating, and preparing the dataset for data analysis.
- Exploring trends and patterns in weekly sales.
- Comparing performance across stores.
- Evaluating relationships between sales and external factors.
  
# Data Source/s
The dataset for this project was downloaded from Kaggle, and the excel sheet have been added to this repository:

The dataset feautures the follong 8 columns:
- Store
- Date
- Weekly Sales
- Holiday Flag
- Temperature
- Fuel Price
- CPI
- Unemployment
  
# Tools
- Microsoft Excel
- Power Query Editor
  
# Data Inspection, Cleaning & Validation

The initial stage of the analysis included 3 phases, which is the data inspection, data cleaning and data validation phases.

## In this phase of the analysis, I performed the following activities:

1. After dowloading the dataset from Kaggle and saving in on my computer, I then opened it in Excel.
2. I checked and verified the structure of the dataset, by counting the total number of rows and columns of the dataset:
   
  - Examples of the Excel functions applied during the data inspection phase:
``` Excel
=COUNT()
=COUNTA()
=COLUMNS()
```
4. Loading the dataset into power query editor.
   
5. Checking, changing and verifying the data types
- This included changing the data type of the 'Date' column, the column included different date formats; Example;
  
- In power query editor, select the 'Add Column' tab and choose 'Custom Column' at the ribbon. Modify the column by adding the following:
  
``` m
Text.SplitAny([Date],"/-,")
```

Add another custom column and editing it as the following: 

  ``` m
[Custom]{2}&"/"&[Custom]{1}&"/"&[Custom]{0}
```
6. Identifying and removing empty or null rows.
   - Using the select and merging the columns, and filtering and deleting all the nulls:
     
  ``` m
#"Inserted Merged Column" = Table.AddColumn(Source, "Merged", each Text.Combine({Text.From([Store], "en-ZA"), [Date], Text.From([Weekly_Sales], "en-ZA"), Text.From([Holiday_Flag], "en-ZA"), Text.From([Temperature], "en-ZA"), Text.From([Fuel_Price], "en-ZA"), Text.From([CPI], "en-ZA"), Text.From([Unemployment], "en-ZA")}, ""), type text),

#"Filtered Rows" = Table.SelectRows(#"Inserted Merged Column", each [Merged] <> null and [Merged] <> ""),

#"Removed Columns" = Table.RemoveColumns(#"Filtered Rows",{"Merged"}),
```
7. Removing duplicated rows from the dataset.
8. Identifiying the outliers, by finding the following per column: 
   - Minimum values
   - Maximum values
   - Total average values
   - Median.

9. Perfoming 

# Exploratory Data Analysis
# Data Analysis
# Analysis Findings
# Recommendations
# Limitations
Walmart stores sales performance data
