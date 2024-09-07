# Slowly changing data (SCD) type 1 & 2 in dbt
Slowly changing data (SCD) refers to when data changes over time.
And this document demonstrate how to update tables with SCD type 1 & 2 in dbt using Incremental materialization & snapshots.

## Incremental materialization for SCD type 1
SCD type 1 is a method to handle evolving data by only keeping the latest change. In database, this can be achieved by merge, upsert (update & insert), delete & insert or overwrite. <br>
In dbt, incremental materialization enables incremental/delta load of data into a table.

### Append new records
For the incremental materialization, `is_incremental()` clause allows dbt to identify new records to be appended to the table.

    {{
        config(
            materialized='incremental'
        )
    }}
    
    select
        id,
        name,
        updated_at
    from {{ ref('dim_student') }}
    
    {% if is_incremental() %}
    where updated_at > (select max(updated_at) from {{ this }})
    {% endif %}

### Late arriving fact
Sometimes, records may arrive late. And having the above `is_incremental()` clause can prevent such records to be missed out.<br>
In such case, the following approaches can be taken:
- Increase filtering range of the updated_at column to allow late arriving records to be inserted. With use of `unique_key` & `incremental_strategy`, explained below, records can be appended in SCD type 1 manner.
  - This can allow dbt to capture any late arriving records. However, this can still miss some late arriving records depending on the filter range.
- Regularly conduct full refresh of the table, to recalibrate the table to sync with the source data.
  - This can ensure the table is fully synced with all late arriving records. However, this can be costly depending on the data size.

### Unique key & Incremental strategy
Additionally, dbt provides a `unique_key` & `incremental_strategy` in configuration for users to update existing records.
`unique_key` config refers to the uniquely identifying column(s) that can be used to identify records to be updated. Note that null value in this can raise an error.

`incremental_strategy` depends on the database adapter as shown below:

|data platform adapter |	append | merge |	delete+insert |	insert_overwrite|
|--|--|--|--|--|
|dbt-postgres |	✅ |	✅ |	✅ |	|
|dbt-redshift |	✅ |	✅ |	✅ |	|
|dbt-bigquery |	 |	✅	 |	 | ✅ |
|dbt-spark |	✅ |	✅ | 	| ✅ |
|dbt-databricks |	✅ |	✅ |	 |	✅ |
|dbt-snowflake |	✅ |	✅ |	✅ |	|
|dbt-trino |	✅ |	✅ |	✅	| |
|dbt-fabric |	✅ |	 |	✅ |	|

Having `incremental_strategy` = 'merge' / 'delete+insert' / 'insert_overwrite' couple with `unique_key` will allow existing records 
to be updated with newest values, achieving SCD type 1.

    {{
        config(
            materialized='incremental',
            incremental_strategy='delete+insert',
            unique_key=['student_id']
        )
    }}
    
    select
        {{ dbt_utils.generate_surrogate_key(['student_id']) }} as sk,
        student_id,
        class_id,
        enrol_date
    from {{ ref('stg_student_enrolment') }}

And it is important to note that the above strategies will behave differently.<br>
For example, `merge` strategy will merge new records into the matching records in the target table.
Meanwhile, `delete+insert` strategy will delete the matching records in the target table first before loading new records.

**Note that these incremental strategies do NOT guarantee uniqueness of the records. Hence, de-duplication needs to be separately handled.**

## SCD type 2

## Reference
- https://docs.getdbt.com/docs/build/incremental-models
- https://docs.getdbt.com/docs/build/incremental-strategy
