# Parquet
- Parquet is a columnar storage format designed to optimise OLAP query performance.
- Data is written/scanned on column-level, reducing analytical processing time compared to row-based transactions.
- Column data can be compressed due to its homogenous nature.
- Supports nested structures using Dremel technique from Google.

## Nested structure in Parquet
Parquet has adoped Dremel technique from Google to enable storing nested structures. This is achieved by storing `Definition level` and `Repetition level` of nested data.

### Definition level
Definition level defines number of optional (=nullable) fields defined in the path of nested column. 

Example nested schema:
- message: Root level of Parquet message
- nested column of contacts.name
```
message phoneBook {
    optional group contacts {
        optional string name;
    }
}
```

Value|Definition level|Note
--|--|--
"contacts": null|0|No optional field exists as first field is null
"contacts": {"name": null}|1|Only the optional field "contacts" is defined whereas field "name" is null
"contacts": {"name": "Travo"}|2|Both "contacts" and "name" are defined

In counting definition level, when a nested column field is not optional (e.g., required), definition level is not counted.

Example nested schema with required field:
```
message exampleLevel {
    optional group a {
        required group b {
            optional string c;
        }
    }
}
```

Value|Definition level|Note
--|--|--
"a": null|0|No optional field exists as first field is null
"a": {"b": null}|X|Impossible as field "b" is required, hence definition level is not counted
"a": {"b": {"c": null}}|1|As optional field "a" is defined. Field "b" is not counted as it's required field, and field "c" is empty
"a": {"b": {"c": "Travo"}}|2|Both optional fields "a" and "c" are defined

Definition level indicates at which nested level the null field occurs. This allows Parquet to only store information where neccessary.

### Repetition level
Repetition level identifies the repeated field in the nested path having repeated values. It indicates the level at which a new list needs to be created.

Example nested lists = [[a,b,c], [d,e]], [[f,g], [h]]:
```
# Schema:
message nestedLists {
    repeated group level 1 {
        repeated string level2;
    }
}

# Data level structure:
{
    level 1: {
        level 2: a,  # Repetition level: 0 as it's new record with no repetion in level 1 or 2
        level 2: b,  # Repetition level: 2 as it's repeated at level 2 (new entry at level 2)
        level 2: c   # Repetition level: 2
    },
    level 1: {
        level 2: d,  # Repetition level: 1 as it's repeated at level 1 (new entry at level 1)
        level 2: e   # Repetition level: 2
    }
}
{
    level 1: {
        level 2: f,  # Repetition level: 0
        level 2: g   # Repetition level: 2
    },
    level 1: {
        level 2: h   # Repetition level: 1
    }
}
```

### How Parquet stores nested structure
Parquet stores nested structure information in repetition level, definition level and value. These information is then used when reconstructing the nested dataset.

For example if we have a nested dataset of {'name': {'first_name': 'Travo'}, {'last_name': 'John'}}, with the following schema:
```
message exampleRecord {
    repeated group name {
        optional string first_name
        optional string last_name
    }
}
```

Parquet would store the nested structure information as:
R|D|Value|Note
--|--|--|--
0|2|'Travo'|R=0 as it's first record in both 'name' & 'last_name'. D=2 as both optional fields of 'name' and 'first_name' are defined
1|2|'John'|R=1 as the record is repeated at 'name' field. D=2 as both optional fields of 'name' and 'last_name' are defined