# SQL Update with a Table
This example demonstrates how Table A can be updated using values from Table B.

    UPDATE
        Table_A
    SET
        Table_A.col1 = Table_B.col1,
        Table_A.col2 = Table_B.col2
    FROM
        Target_Table AS Table_A
        INNER JOIN Source_Table AS Table_B
            ON Table_A.id = Table_B.id
    WHERE
        Table_A.col3 = 'cool'
