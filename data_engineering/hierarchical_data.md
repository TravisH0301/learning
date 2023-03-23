# Hierarchical Data
- [Adjacency List Design](#adjacency-list-design)
- [Recursive Queries](#recursive-queries)
- [Implementation](#implementation)

Hierarchical data has a parent-child relationship, where each child can have multiple children of its own. <br>
To represent hierarchical data in a database, we can use the adjacency list design.

## Adjacency List Design
The adjacency list is a design method for storing hierarchical data in a table by adding a column to the table that refers to the parent record's primary key. This column is regarded as the foreign key and it creates a link between the child record and its parent record.

Here's an example data table that uses the adjacency list design for an employee hierarchy:

![image](https://user-images.githubusercontent.com/46085656/227180540-3dead881-7f34-4a15-9520-0bab2ac2b976.png)

The `employee` table has four columns: `id`, `name`, `role`, and `manager_id`. <br>
The `id` column is the primary key, which uniquely identifies each employee. <br>
The `manager_id` column is a foreign key that refers to the `id` column of another employee in the same table, representing the employee's manager. <br>
Note there is an one-to-many relationship between `id` and `manager_id`. For every unique `id`, there are many `manager_id`.

Here's an example dataset for the `employee` table:

id | name | role | manager_id
--|--|--|--
1 | Alice | CEO |
2 | Bob | Head of Sales | 1
3 | Charlie | Salesperson | 2
4 | David | Salesperson | 2
5 | Emily | Head of Support | 1
6 | Frank | Customer Support | 5
7 | Grace | Customer Support | 5
8 | Henry | Customer Support | 5
  
In this example, Alice is the CEO (root node) of the employee hierarchy. <br>
Charlie and David report to Bob, while Frank, Grace and Henry report to Emily. <br>
The `manager_id` column shows the `id` of each employee's manager, which forms the hierarchical structure of the data.

## Recursive Queries
To query hierarchical data stored in the `employee` table, we can use a recursive query. A recursive query is a query that refers to itself, allowing us to traverse the hierarchical data and retrieve the data in a hierarchical manner.

Here's an example of a recursive query that retrieves all employees and their manager in hierarchical order:

```sql
WITH RECURSIVE employee_hierarchy AS (
  -- Anchor query with the root node
  SELECT id, name, role, manager_id, 1 as level
  FROM employee
  WHERE manager_id IS NULL
  UNION ALL
  -- Recursive query adding records the CTE
  SELECT e.id, e.name, e.role, e.manager_id, level + 1
  FROM employee e
  INNER JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT id, name, role, level
FROM employee_hierarchy
ORDER BY level, id;
```

This query uses a common table expression (CTE) to create a employee_hierarchy table that contains all employees and their manager in hierarchical order. <br>
The CTE contains two parts: an anchor query that selects the root node (the CEO) and a recursive query that selects all remaining nodes and connects them to their parent node. <br>
The `level` column indicates the level of each employee in the hierarchy.

Here's an example result set for the above query based on the example data table:

id | name | role | level 
--|--|--|--
1 | Alice   | CEO                  |     1
2 | Bob     | Head of Sales        |     2
5 | Emily   | Head of Support      |     2
3 | Charlie | Salesperson          |     3
4 | David   | Salesperson          |     3
6 | Frank   | Customer Support     |     3
7 | Grace   | Customer Support     |     3
8 | Henry   | Customer Support     |     3
  
This result set shows all employees and their level in the hierarchy, ordered by their level and id.

In conclusion, the adjacency list design and recursive queries are powerful tools for representing and querying hierarchical data in a database.

## Implementation
```sql
create table prd_app_bot_analytics_disco.employee_hier (
	id int,
	"name" varchar(50),
	"role" varchar(50),
	manager_id int
);

insert into prd_app_bot_analytics_disco.employee_hier
(1, 'Alice', 'CEO', null);
insert into prd_app_bot_analytics_disco.employee_hier
(2, 'Bob', 'Head of Sales', 1);
insert into prd_app_bot_analytics_disco.employee_hier
(3,	'Charlie', 'Salesperson', 2);
insert into prd_app_bot_analytics_disco.employee_hier
(4, 'David', 'Salesperson', 2);
insert into prd_app_bot_analytics_disco.employee_hier
(5,	'Emily', 'Head of Support',	1);
insert into prd_app_bot_analytics_disco.employee_hier
(6, 'Frank', 'Customer Support', 5);
insert into prd_app_bot_analytics_disco.employee_hier
(7, 'Grace', 'Customer Support', 5);
insert into prd_app_bot_analytics_disco.employee_hier
(8, 'Henry', 'Customer Support', 5);

select * from prd_app_bot_analytics_disco.employee_hier;
```

![image](https://user-images.githubusercontent.com/46085656/227188167-0467d56d-a8e1-4722-a569-9c1fd4cef9be.png)

```sql
with recursive temp as (
	select id, "name", "role", manager_id, 1 as "level"
	from prd_app_bot_analytics_disco.employee_hier
	where id = 1
	union all
	select emp.id, emp."name", emp."role", emp.manager_id, temp."level" + 1
	from prd_app_bot_analytics_disco.employee_hier emp
	inner join temp temp
		on emp.manager_id = temp.id
)

select id, "name", "role", "level"
from temp
order by "level", id;
```

![image](https://user-images.githubusercontent.com/46085656/227188383-55a16900-af9c-440d-8763-90a0725d1900.png)
