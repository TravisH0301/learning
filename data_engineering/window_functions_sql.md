# Windows Functions in SQL
A window function performs a calculation on rows. Unlike the aggregate function (GROUP BY), the rows are not grouped into a single output row. Rows retain their separate identities.<br>
The window function can be used using OVER (PARTITION BY <column> ORDER BY <column> ACS|DESC).<br>

## Contents
- [Running Total](#Running-Total)
- [Partitioned Running Total](#Partitioned-Running-Total)
- [Ranking Rows](#Ranking-Rows)
- [Aggregations](#Aggregations)
- [Window Function Alias](#Window-Function-Alias)
	
### Running Total
    /*
    Running total of standard_qty is calculated over occurred_at with ascending order.
    */
    SELECT occurred_at,
           standard_qty,
           SUM(standard_qty) OVER (ORDER BY occurred_at ASC) AS "running_total"
    FROM orders;
    
|occurred_at|standard_qty|running_total|
|--|--|--|
|2013-12-04T04:22:44.000Z|0|0
|2013-12-04T04:45:54.000Z|490|490
|2013-12-04T04:53:25.000Z|528|1018

### Partitioned Running Total
    /*
    Annual Running total of standard_qty is calculated over occurred_at partitioned by years with ascending order.
    */
    SELECT occurred_at,
	       DATE_TRUNC('YEAR', occurred_at) AS year,
	       standard_qty,
           SUM(standard_qty) OVER (PARTITION BY DATE_TRUNC('YEAR', occurred_at) ORDER BY occurred_at ASC) AS annual_running_total
    FROM orders
    
occurred_at|	year|	standard_qty|	annual_running_total
--|--|--|--
2013-12-31T02:04:00.000Z|	2013-01-01T00:00:00.000Z|	79|	79
2013-12-31T12:58:47.000Z|	2013-01-01T00:00:00.000Z|	119|	198
2013-12-31T13:14:55.000Z|	2013-01-01T00:00:00.000Z|	485|	683
2014-01-01T10:56:08.000Z|	2014-01-01T00:00:00.000Z|	515|	515
2014-01-01T11:13:13.000Z|	2014-01-01T00:00:00.000Z|	0|	515
2014-01-01T13:11:47.000Z|	2014-01-01T00:00:00.000Z|	37|	552

### Ranking Rows
    /*
    Rows are ranked over total for each account_id.
    Note that RANK() will skip a number if duplicated ranks are found.
    ex) 1 > 2 > 2 > 4
    DENSE_RANK() can be used if skipping is not wanted.
    ex) 1 > 2 > 2 > 3
    ROW_NUMBER() can be used if duplication is not wanted. 
    Note, this may not give accurate representation if orders are important!
    ex) 1 > 2 > 3 > 4
    */
    SELECT id,
           account_id,
           total,
           RANK() OVER (PARTITION BY account_id ORDER BY total DESC) AS total_rank
    FROM orders
    
id|	account_id|	total|	total_rank
--|--|--|--
25|	1041|	395|	1
26|	1041|	329|	2
27|	1041|	212|	3
4327|	1051|	648|	1
31|	1051|	595|	2
28|	1051|	589|	3

### Aggregations
    /*
    Aggregations are made for standard_qty per month for each account_id.
    Note that for the rows with identical ranks, the aggregations are given at the lastest row.
    ex) row 1 | rank 1 | qty=100 | sum=150
        row 2 | rank 1 | qty=50  | sum=150
    */
    SELECT id,
           account_id,
           DATE_TRUNC('month', occurred_at) AS month,
           DENSE_RANK() OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)) AS dense_rank,
           standard_qty,
           SUM(standard_qty) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)) AS sum_std_qty,
           COUNT(standard_qty) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)) AS count_std_qty,
           ROUND(AVG(standard_qty) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)),2) AS avg_std_qty,
           MIN(standard_qty) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)) AS min_std_qty,
           MAX(standard_qty) OVER (PARTITION BY account_id ORDER BY DATE_TRUNC('month',occurred_at)) AS max_std_qty
    FROM orders
    
id|	account_id|	month|	dense_rank|	standard_qty|	sum_std_qty|	count_std_qty|	avg_std_qty|	min_std_qty|	max_std_qty
--|--|--|--|--|--|--|--|--|--
1|	1001|	2015-10-01T00:00:00.000Z|	1|	123|	123|	1|	123.00|	123|	123
4307|	1001|	2015-11-01T00:00:00.000Z|	2|	506|	819|	3|	273.00|	123|	506
2|	1001|	2015-11-01T00:00:00.000Z|	2|	190|	819|	3|	273.00|	123|	506
3|	1001|	2015-12-01T00:00:00.000Z|	3|	85|	1430|	5|	286.00|	85|	526
4308|	1001|	2015-12-01T00:00:00.000Z|	3|	526|	1430|	5|	286.00|	85|	526
4309|	1001|	2016-01-01T00:00:00.000Z|	4|	566|	2140|	7|	305.71|	85|	566
4|	1001|	2016-01-01T00:00:00.000Z|	4|	144|	2140|	7|	305.71|	85|	566
    
### Window Function Alias
Alias can be set up for a window function using WINDOW clause if it's used multiple times.<br>
Note that the clause needs to be between WHERE and ORDER BY.

    /*
    Window function alias is defined using WINDOW clause.
    */
    SELECT id,
           account_id,
           DATE_TRUNC('year',occurred_at) AS year,
           DENSE_RANK() OVER main_window AS dense_rank,
           total_amt_usd,
           SUM(total_amt_usd) OVER main_window AS sum_total_amt_usd,
           COUNT(total_amt_usd) OVER main_window AS count_total_amt_usd,
           AVG(total_amt_usd) OVER main_window AS avg_total_amt_usd,
           MIN(total_amt_usd) OVER main_window AS min_total_amt_usd,
           MAX(total_amt_usd) OVER main_window AS max_total_amt_usd
    FROM orders
    WINDOW main_window AS (PARTITION BY account_id ORDER BY DATE_TRUNC('year',occurred_at));
