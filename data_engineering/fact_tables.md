# Fact Tables
Fact tables are the foundation in dimensional data modeling. They contain metrics (sometimes not - e.g., factless fact table) and are
surrounded by dimensions that provide additional information via table joins.

The first step in designing a fact table is to declare the grain. The grain is the business definition of what a single fact table record represents.
For example, in the transactional fact table of the retail business, each record represents a physical transaction in the business.

## Transactional Fact Table
- Captures data at the lowest level of details.
- Records provide metrics about each transaction.
- Example: Recording every sale transaction in a retail store.

| Transaction_ID | Product_ID | Customer_ID | Store_ID | Amount | Date       |
|----------------|------------|-------------|----------|--------|------------|
| 1              | 101        | 501         | 401      | 120.00 | 2024-01-01 |
| 2              | 102        | 502         | 402      |  60.00 | 2024-01-01 |
| 3              | 103        | 503         | 403      |  45.00 | 2024-01-01 |
| 4              | 104        | 504         | 404      |  85.00 | 2024-01-02 |
| 5              | 105        | 505         | 405      |  50.00 | 2024-01-02 |

## Periodic Snapshot Fact Table
- Captures data at regular intervals, like daily, weekly, or monthly.
- Requires a transaction table (either OLTP or OLAP) to aggregate data at regular intervals.
- Useful for analyzing trends over a set period.
- Example: Summarizing total sales and expenses at the end of each week.

| Week_Ending    | Store_ID | Total_Sales | Total_Customers |
|----------------|----------|-------------|-----------------|
| 2024-01-07     | 401      | 1250.00     | 250             |
| 2024-01-07     | 402      | 1150.00     | 230             |
| 2024-01-14     | 401      | 1500.00     | 300             |
| 2024-01-14     | 402      | 1400.00     | 280             |
| 2024-01-21     | 401      | 1600.00     | 320             |

## Accumulating Snapshot Fact Table
- Tracks the process life cycle of a record from start to end.
- Requires a transaction records to create this accumulating snapshot fact table.
- Each record is updated when there is change in the process life cycle (e.g., delivery is completed).
- Useful for processes with a clear beginning and end.
- Example: Tracking an order record from placement to delivery.

| Order_ID | Order_Date | Ship_Date | Delivery_Date | Total_Value |
|----------|------------|-----------|---------------|-------------|
| 1001     | 2024-01-01 | 2024-01-03| 2024-01-06    |  300.00     |
| 1002     | 2024-01-02 | 2024-01-04| 2024-01-07    |  250.00     |
| 1003     | 2024-01-03 | 2024-01-05| 2024-01-08    |  150.00     |
| 1004     | 2024-01-04 | 2024-01-06| 2024-01-09    |  350.00     |
| 1005     | 2024-01-05 | 2024-01-07| 2024-01-10    |  200.00     |

## Factless Fact Table
- Contains keys to dimension tables but no numeric or factual data.
- Useful for tracking events or occurrences without any measures or numeric values.
- Example: Tracking student attendance in classes without storing specific data about the attendance.

| Student_ID | Course_ID | Attendance_Date |
|------------|-----------|-----------------|
| 2001       | 3001      | 2024-02-01      |
| 2002       | 3002      | 2024-02-01      |
| 2003       | 3003      | 2024-02-01      |
| 2004       | 3004      | 2024-02-02      |
| 2005       | 3005      | 2024-02-02      |

## Reduced Fact Table
- A simplified version of a comprehensive fact table.
- Created for performance optimization or to focus on specific business needs.
- Can be created by joining multiple fact tables with subset of required columns.
- Example: A fact table that only includes key metrics for a high-level business overview.

| Month      | Product_Category | Region | Total_Sales |
|------------|------------------|--------|-------------|
| 2024-01    | Electronics      | North  | 15000.00    |
| 2024-01    | Apparel          | South  | 12000.00    |
| 2024-02    | Electronics      | North  | 18000.00    |
| 2024-02    | Apparel          | South  | 14000.00    |
| 2024-03    | Electronics      | North  | 20000.00    |
