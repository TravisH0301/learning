# Windows Functions in SQL

## Examples
### Running Total
    /*
    Running total of standard_qty is calculated over occurred_at with the given descending order.
    */
    select occurred_at,
           standard_qty,
           sum(standard_qty) over (order by occurred_at desc) as "running_total"
    from orders;
    
|occurred_at|standard_qty|running_total|
|--|--|--|
|2017-01-02T00:02:40.000Z|42|42|
|2017-01-01T23:50:16.000Z|291|333|
|2017-01-01T22:29:50.000Z|38|371|
