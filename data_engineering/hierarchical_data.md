## Hierarchical Data Tables and Recursive Queries

Hierarchical data is data that has a parent-child relationship, where each child can have multiple children of its own. This type of data is commonly found in organizational charts, file systems, and product categories. To represent hierarchical data in a database, we can use the adjacency list design.

### Adjacency List Design

The adjacency list design is a method for storing hierarchical data in a table by adding a column to the table that refers to the parent record's primary key. This column is known as the foreign key and it creates a link between the child record and its parent record.

Here's an example data table that uses the adjacency list design for an employee hierarchy:

/*-- Diagram --*/


The `employee` table has four columns: `id`, `name`, `role`, and `manager_id`. The `id` column is the primary key, which uniquely identifies each employee. The `name` and `role` columns store the name and role of each employee, respectively. The `manager_id` column is a foreign key that refers to the `id` column of another employee in the same table, representing the employee's manager. If an employee doesn't have a manager (e.g., the CEO), the `manager_id` column can be set to `NULL`.

Here's an example dataset for the `employee` table:

id | name | role | manager_id
----+---------+----------------------+------------
1 | Alice | CEO |
2 | Bob | Head of Sales | 1
3 | Charlie | Salesperson | 2
4 | David | Salesperson | 2
5 | Emily | Head of Support | 1
6 | Frank | Customer Support | 5
7 | Grace | Customer Support | 5
8 | Henry | Customer Support | 5
  

In this example, Alice is the CEO (root node) of the employee hierarchy, while Bob and Emily are the heads of Sales and Support, respectively. Charlie and David report to Bob, while Frank, Grace, and Henry report to Emily. The `manager_id` column shows the `id` of each employee's manager, which forms the hierarchical structure of the data.

### Recursive Queries

To query hierarchical data stored in the `employee` table, we can use a recursive query. A recursive query is a query that refers to itself, allowing us to traverse the hierarchical data and retrieve the data in a hierarchical manner.

Here's an example of a recursive query that retrieves all employees and their manager in hierarchical order:

```sql
WITH RECURSIVE employee_hierarchy AS (
  SELECT id, name, role, manager_id, 1 as level
  FROM employee
  WHERE manager_id IS NULL
  UNION ALL
  SELECT e.id, e.name, e.role, e.manager_id, level + 1
  FROM employee e
  JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT id, name, role, level
FROM employee_hierarchy
ORDER BY level, id;


This query uses a common table expression (CTE) to create a employee_hierarchy table that contains all employees and their manager in hierarchical order. The CTE contains two parts: an anchor query that selects the root node (the CEO) and a recursive query that selects all remaining nodes

and connects them to their parent node. The employee_hierarchy table contains four columns: id, name, role, and level. The id, name, and role columns are self-explanatory, while the level column indicates the level of each employee in the hierarchy.

Here's an example result set for the above query based on the example data table:

id |   name  |         role          | level 
----+---------+----------------------+-------
  1 | Alice   | CEO                  |     1
  2 | Bob     | Head of Sales        |     2
  5 | Emily   | Head of Support      |     2
  3 | Charlie | Salesperson          |     3
  4 | David   | Salesperson          |     3
  6 | Frank   | Customer Support     |     3
  7 | Grace   | Customer Support     |     3
  8 | Henry   | Customer Support     |     3
  
This result set shows all employees and their level in the hierarchy, ordered by their level and id.

In conclusion, the adjacency list design and recursive queries are powerful tools for representing and querying hierarchical data in a database. By storing the parent-child relationship in the table and using a recursive query, we can easily traverse the hierarchy and retrieve the data in a hierarchical manner.
