# Slowly changing data (SCD) type 1 & 2 in dbt
Slowly changing data (SCD) refers to when data changes over time.
And this document demonstrate how to update tables with SCD type 1 & 2 in dbt using Incremental materialization & snapshots.

## Incremental materialization for SCD type 1
SCD type 1 is a method to handle evolving data by only keeping the latest change. In database, this can be achieved by merge, upsert (update & insert), delete & insert or overwrite.<br>
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
SCD type 2 is a method to keep both historical data with latest data in a table with the use of additional columns such as, valid_from, valid_to & is_valid - indicating validity of the records.<br>
In dbt, snapshots can be used to keep track of data evolution over time.

### Snapshots for SCD type 2
In dbt, snapshots mechanism takes a snapshot of the query output and tracks the changes in between snapshot runs. The snapshot file contains a select statement within a snapshot block, and is typically stored under `snapshots` directory.

        {% snapshot dim_student_snapshot %}
        
        {{
            config(
                target_schema='snapshots',
                unique_key='id',
                strategy='check',
                check_cols='all'
            )
        }}
        
        select * from {{ ref('stg_student') }}
        
        {% endsnapshot %}

When the above snapshot is triggered (via dbt snapshot or dbt built), the output of the select statement will be stored as a table in the `target_schema` (dbt recommends a separate schema to keep snapshot tables) with additional columns - `dbt_scd_id`, `dbt_updated_at`, `dbt_valid_from`, `dbt_valid_to` (null if latest). And the changes will be tracked based on the configured snapshot `strategy`.

### Timestamp strategy to track changes
The `timestamp` strategy looks at a timestamp column in the source data to track changes. If the timestamp column of a certain record in source data is more recent than the matching record in the previous snapshot, dbt will invalidate the old record and insert the new record. If the timestamp is unchanged, no change will be made.

This strategy requires a timestampe column to be defined with `updated_at` in the configuration.

        {{
            config(
              target_schema='snapshots',
              strategy='timestamp',
              unique_key='id',
              updated_at='updated_at',
            )
        }}

### Check strategy to track changes
When the source data doesn't have a reliable timestamp column, `check` strategy can be used by comparing a list of columns between current and previous snapshots of the records.
If any of the column value has changed, dbt will invalidate the old records and insert the new records. If none changed, no change will be made.

This strategy requires the columns to be defined with `check_cols` in the configuration for comparison. `'all'` can be used to compare all columns, except the unique key.

        {{
            config(
              target_schema='snapshots',
              strategy='check',
              unique_key='id',
              check_cols=['status', 'is_cancelled'],
            )
        }}

### Sanpshot best practices
- Use `timestamp` strategy if possible for better coping with column addition & deletion
- Ensure `unique_key` is really unique
- Store snapshots separate to analytics schema to protect snapshots against accidental change
- Avoid having business logic to prevent any impact from business logic change

## Reference
- https://docs.getdbt.com/docs/build/incremental-models
- https://docs.getdbt.com/docs/build/incremental-strategy
- https://docs.getdbt.com/docs/build/snapshots
