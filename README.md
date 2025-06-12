# DA-Task7

 1. Load SQLite Database

import sqlite3
conn = sqlite3.connect("sales_data.db")

Explanation (in "I" form):
I started by importing the sqlite3 library and connecting to a local SQLite database named sales_data.db. This allowed me to interact with the database directly from Python.

2. Create and Populate the Sales Table

cursor = conn.cursor()

cursor.execute("DROP TABLE IF EXISTS sales")

cursor.execute("""
CREATE TABLE sales (
    id INTEGER PRIMARY KEY,
    date TEXT,
    product TEXT,
    category TEXT,
    region TEXT,
    quantity INTEGER,
    price REAL
)
""")

sample_data = [
    ('2024-06-01', 'Laptop', 'Electronics', 'North', 5, 750.0),
    ('2024-06-01', 'Phone', 'Electronics', 'East', 8, 500.0),
    ('2024-06-02', 'Tablet', 'Electronics', 'West', 3, 300.0),
    ('2024-06-03', 'Monitor', 'Electronics', 'South', 6, 200.0),
    ('2024-06-03', 'Keyboard', 'Accessories', 'North', 15, 40.0),
    ('2024-06-04', 'Mouse', 'Accessories', 'East', 20, 25.0),
    ('2024-06-05', 'Laptop', 'Electronics', 'West', 4, 750.0),
    ('2024-06-05', 'Tablet', 'Electronics', 'South', 2, 300.0),
    ('2024-06-06', 'Phone', 'Electronics', 'North', 7, 500.0),
    ('2024-06-07', 'Monitor', 'Electronics', 'East', 5, 200.0),
    ('2024-06-07', 'Mouse', 'Accessories', 'West', 10, 25.0),
]

cursor.executemany("""
INSERT INTO sales (date, product, category, region, quantity, price) 
VALUES (?, ?, ?, ?, ?, ?)
""", sample_data)

conn.commit()

Explanation:
I created a sales table with fields like product, category, region, quantity, and price. Then, I inserted multiple rows of sample sales data to make the analysis meaningful and realistic.

3. Run SQL Query 1 – Product-wise Summary

import pandas as pd

query1 = """
SELECT 
    product,
    category,
    region,
    SUM(quantity) AS total_quantity,
    ROUND(SUM(quantity * price), 2) AS total_revenue,
    COUNT(*) AS transactions
FROM sales
GROUP BY product, category, region
ORDER BY total_revenue DESC
"""

df1 = pd.read_sql_query(query1, conn)
print("Sales Summary by Product, Category & Region:\n")
print(df1)

Explanation:
I wrote an SQL query that groups sales by product, category, and region, then calculates total quantity sold, total revenue (quantity * price), and number of transactions.
I used pandas to load the query result into a DataFrame and printed it to get a detailed breakdown of product-level sales across regions.

4. Plot Bar Chart of Revenue per Product

import matplotlib.pyplot as plt

chart_data = df1.groupby('product')['total_revenue'].sum().reset_index()

chart_data.plot(kind='bar', x='product', y='total_revenue', legend=False, color='coral')

plt.title("Total Revenue by Product")
plt.xlabel("Product")
plt.ylabel("Revenue")
plt.tight_layout()
plt.show()

Explanation:
I grouped the total revenue by product and used matplotlib to plot a bar chart showing revenue per product.
This helped me visualize which products are generating the most income.

5. Run SQL Query 2 – Daily Sales Trend

query2 = """
SELECT 
    date,
    SUM(quantity * price) AS daily_revenue,
    SUM(quantity) AS daily_quantity
FROM sales
GROUP BY date
ORDER BY date
"""

df2 = pd.read_sql_query(query2, conn)
print("\nDaily Sales Summary:\n")
print(df2)

Explanation:
I created a second SQL query to calculate total daily revenue and quantity sold. This helped me understand how the business performed on different days.
I used GROUP BY date and ORDER BY date to make the output more readable in chronological order.

6. Plot Line Chart of Daily Revenue

df2.plot(kind='line', x='date', y='daily_revenue', marker='o', color='green')

plt.title("Daily Revenue Trend")
plt.xlabel("Date")
plt.ylabel("Revenue")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

Explanation:
I plotted a line chart showing how revenue changed day by day. This gave me a clear picture of sales performance trends over the week.
