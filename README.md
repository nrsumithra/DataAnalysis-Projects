# DataAnalysis-Projects

I. Sales Data Insight

Download SQL Database db_dump.sql file to your local computer and import the file to your SQL Workbench.
Data Analysis using SQL.

Use database Sales

use sales;

Show all customer records

SELECT * FROM customers;

Show total number of customers

SELECT count(*) FROM customers;

Show transactions for Chennai market (market code for chennai is Mark001

SELECT * FROM transactions where market_code='Mark001';

Show distrinct product codes that were sold in chennai

SELECT distinct product_code FROM transactions where market_code='Mark001';

Show transactions where currency is US dollars

SELECT * from transactions where currency="USD"

Show transactions where the sales amount is <= 0- this needs to be removed in power Bi

SELECT COUNT(*) FROM TRANSACTIONS WHERE SALES_AMOUNT <= 0 ;

Show sales transactions by Zone

SELECT MARKET_CODE, COUNT( MARKET_CODE) AS 'SALES BY ZONE', T.*, D.* FROM TRANSACTIONS T INNER JOIN DATE D ON T.ORDER_DATE = D.DATE GROUP BY MARKET_CODE ORDER BY COUNT(MARKET_CODE) DESC ;

Show transactions in 2020 join by date table and markets table

SELECT MARKET_CODE,COUNT(MARKET_CODE) AS 'SALES BY CITY',T.product_code,T.customer_code,T.sales_qty,T.sales_amount,T.currency,D.date,D.year,D.month_name, M.MARKETS_NAME 
FROM TRANSACTIONS T 
	  INNER JOIN DATE D ON T.ORDER_DATE = D.DATE 
    INNER JOIN MARKETS M ON T.MARKET_CODE = M.MARKETS_CODE
    WHERE YEAR = 2020
GROUP BY MARKET_CODE ORDER BY T.SALES_AMOUNT DESC ;

Show total revenue in year 2020,

SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";

Show total revenue in year 2020, January Month,

SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");

Show total revenue in year 2020 in Chennai

SELECT MARKET_CODE, SUM(SALES_AMOUNT) AS 'TOTAL SALES', D.YEAR,M.MARKETS_NAME  FROM TRANSACTIONS T 
    INNER JOIN DATE D  ON T.ORDER_DATE = D.DATE
    INNER JOIN MARKETS M ON T.MARKET_CODE = M.MARKETS_CODE    
WHERE MARKETS_NAME='CHENNAI' AND D.YEAR = 2020;

Data Analysis Using Power BI

Import Database 'sales'.

Check for the connections between the tables

In the power query editor 

Transform Data: Sales Transactions table

Filter those records where the sales amount is >0.

= Table.SelectRows(sales_transactions, each ([sales_amount] > 0))

Filter those records with USD#(cr) and INR#(cr) alone.

= Table.SelectRows(#"records with sales_amount >0", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))

Convert USD to INR.

= Table.AddColumn(#"filter INR/r and USD/r records", "final_sales_amount", each if [currency] = "USD#(cr)" then [sales_amount]*75 else [sales_amount])

Transform Data  : Sales Markets Table

 Remove null value records
 
 = Table.SelectRows(sales_markets, each ([zone] <> ""))
