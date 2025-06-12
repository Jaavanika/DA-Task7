1. Load SQLite Database

I started by importing the sqlite3 library and connecting to a local SQLite database named sales_data.db. This allowed me to interact with the database directly from Python.

2. Create and Populate the Sales Table

I created a sales table with fields like product, category, region, quantity, and price. Then, I inserted multiple rows of sample sales data to make the analysis meaningful and realistic.

3. Run SQL Query 1 – Product-wise Summary

I wrote an SQL query that groups sales by product, category, and region, then calculates total quantity sold, total revenue (quantity * price), and number of transactions.
I used pandas to load the query result into a DataFrame and printed it to get a detailed breakdown of product-level sales across regions.

4. Plot Bar Chart of Revenue per Product

I grouped the total revenue by product and used matplotlib to plot a bar chart showing revenue per product.
This helped me visualize which products are generating the most income.

5. Run SQL Query 2 – Daily Sales Trend

I created a second SQL query to calculate total daily revenue and quantity sold. This helped me understand how the business performed on different days.
I used GROUP BY date and ORDER BY date to make the output more readable in chronological order.

6. Plot Line Chart of Daily Revenue

I plotted a line chart showing how revenue changed day by day. This gave me a clear picture of sales performance trends over the week.

