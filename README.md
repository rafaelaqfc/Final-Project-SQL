# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
This project has the main purpose to practice data cleaning, tranforming and analysing data in SQL. 

## Process
Steps to follow:

1. Extract data (import the spreadsheets of 5 tables from ecommerce database to PgAdmin)
all_sessions spreadsheet:

15135 rows in the all_sessions spreadsheet 
33 columns

productRefundAmount empty
productQuantity empty
productReveneu empty
itemQuantity empty
itemRevenue empty
transactionRevenue
transactionId
eCommerceAction_option

analytics spreadsheet

14 columns
1048575

userid empty
units_sold empty
timeonsite empty
revenue empty

products spreadsheet

7 columns
1093 rows
no column has a null value or is empty

sales_by_sku

2 columns
463 rows
no column has a null value or is empty

sales_record

8 columns
455 rows

As we know, just having a look at the results of my queries is not enough to confirm that our data is correct to be analyzed. So, our QA (Quality Assurance) strategies involved:
  
I started by cleaning up the table names by renaming them with underscores and making them consistently.

2. Clean, transform and analyze data
First of all, I had a look at the raw data in the spreadsheets to see how it was organized, formatted and structured. It was checked the columns and rows and if they were consistently organized (e.g. such as the number of columns and their data type and which ones were filled out against the ones were with missing values or null). Also, it was removed double and single quotes and columsn renamed with underscores in order to have more readability. This means that during this first step I spend some time exploring the raw data and doing a bit of cleaning before importing the files in the .csv-UTF8 file and loading the data in my database. Also, all the tables were created with scripting SQL.

UTF8csv file converted

Second, I created some hypothesis of my research based on what is my main interest on this data. 
In the second place, 


3. Load data into a database

4, Develop and implement a QA process to validate transformed data against raw data
So, checking the number of columns and names after running a query, performing some statistical analysis were some of the strategies we used as part of our QA, writing desciptive comments before running our queries and finally making the queries readable (uppercased keywords, use aliases for columns, witespaces, tried identation and quering queries in separated blocks), testing unit queries separated first and then integrated with others afterwards. Also, those questions were posed as part of the QA process:

Are the results expected? Are the count of columns and rowns expected? Also, are my counts before and after joining tables expected and checked with the tables count originally?
Have any of my primary keys been duplicated?
Are my JOIN fields in the same data type? Are my tables with the same number of columns before joining them?
Are my CASE WHEN statements working as intended?
Does the results after cleaning and transforming the data making sense?
Are there rowms owith NULLs value?



## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
(discuss challenges you faced in the project)

## Future Goals
(what would you do if you had more time?)
