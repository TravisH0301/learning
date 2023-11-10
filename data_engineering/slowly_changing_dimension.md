# Slowly Changning Dimension (SCD)
Over time, data source may change and this change may need to get implemented on a data warehouse. <br>
Slowly Changing Dimension (SCD) is a dimension that manages both current and historical data over time in a data warehouse.

There are several types of SCDs to manage the changing source data over time. Among them, Type 1 and 2 SCDs are most widely used.

## Table of Contents
- [Type 0 SCD (No Change)](#type-0-scd-no-change)
- [Type 1 SCD (Latest Record Only)](#type-1-scd-latest-record-only)
- [Type 2 SCD (Maintains History)](#type-2-scd-maintains-history)
- [Type 3 SCD](#type-3-scd)
- [Type 4 SCD](#type-4-scd)
- [Type 6 SCD (1+2+3)](#type-6-scd-123)

## Type 0 SCD (No Change)
Type 0 SCD is when we don't make any changes to the dimension. This is usually the case when the changed data is no longer relevant or important. Hence, the dimension is static.

## Type 1 SCD (Latest Record Only)
In type 1 SCD, we only maintain the latest snapshot of the data by overwriting the data. And we don't maintain the data change history. <br>
This type is useful when only the up-to-date data is important to the business operation.

## Type 2 SCD (Maintains History)
In type 2 SCD, the data change history is managed by creating additional columns in the data warehouse. Then rows are added whenever the data source changes.
For example,

Before:<br>
Source system:
id|name|office|date effective
--|--|--|--
123|Tom|Sydney|2020-01-01

Data warehouse table:
id|name|office|from date|to date|current flat
--|--|--|--|--|--
123|Tom|Sydney|2020-01-01|9999-12-31|Y

After:<br>
Source system:
id|name|office|date effective
--|--|--|--
123|Tom|Melbourne|2020-06-12

Data warehouse table:
id|name|office|from date|to date|current flat
--|--|--|--|--|--
123|Tom|Sydney|2020-01-01|2020-06-11|N
123|Tom|Melbourne|2020-06-12|9999-12-31|Y

## Type 3 SCD
In type 3 SCD, the change in data is managed by creating additional columns, yet no rows are added to the dimension. 
Because no row is added, some historical data is going to be lost.
For example:

Before:<br>
Source system:
id|name|office|date effective
--|--|--|--
123|Tom|Sydney|2020-01-01

Data warehouse table:
id|name|office|date effective
--|--|--|--
123|Tom|Sydney|2020-01-01

After:<br>
Source system:
id|name|office|date effective
--|--|--|--
123|Tom|Melbourne|2020-06-12

Data warehouse table:
id|name|previous office|current office|date effective
--|--|--|--|--
123|Tom|Sydney|Melbourne|2020-06-12

## Type 4 SCD 
The type 4 SCD is a combination of type 1 and 2 SCDs. Two dimension tables are used, where one is for keeping the up-to-date data (type 1 SCD), and another table is for
keep the historical data (type 2 SCD).

## Type 6 SCD (1+2+3)
The type 6 SCD is a combination of type 1, 2 and 3 SCDs. A single dimension table is used as below:

Data warehouse table:
id|name|history office|current office|from date|to date|current flag
--|--|--|--|--|--|--
123|Tom|Sydney|Melbourne|2020-01-01|2020-06-11|N
123|Tom|Melbourne|Melbourne|2020-06-12|9999-12-31|Y

The history office column refers to the office that was used between from date and to date columns - for historical purpose. <br>
Whereas, current office column will always represent the up-to-date data regardless of the rows.
