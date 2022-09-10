# SQL Join
- [Inner Join](#inner-join)
- [Left (Outer) Join](#left-outer-join)
- [Right (Outer) Join](#right-outer-join)
- [Full (Outer) Join](#full-outer-join)
- [Anti-Join](#anti-join)
- [Cross Join](#cross-join)

This document shows examples for SQL joins. And the below tables are used in the examples.
<!--
	-- Create table Customers
	CREATE TABLE CUSTOMERS (
		CUST_ID INT,
		CUST_NAME VARCHAR(10),
		ORDER_ID INT
	);

	-- Create table Orders
	CREATE TABLE ORDERS (
		ORDER_ID INT,
		ORDER_DESC VARCHAR(10)
	);

	-- Populate table Customers
	INSERT INTO CUSTOMERS VALUES (1, "John", 100);
	INSERT INTO CUSTOMERS VALUES (1, "John", 100);
	INSERT INTO CUSTOMERS VALUES (2, "David", 200);
	INSERT INTO CUSTOMERS VALUES (3, "Chris", NULL);

	-- Populate table Orders
	INSERT INTO ORDERS VALUES (100, "Bread");
	INSERT INTO ORDERS VALUES (200, "Cookie");
	INSERT INTO ORDERS VALUES (NULL, "Milk");
-->

<img width=400px src="https://user-images.githubusercontent.com/46085656/189480846-aaa73524-d731-4cac-9230-981ca290a969.png">

## Inner Join
	-- Inner join
	SELECT
		C.*,
		O.*
	FROM CUSTOMERS C
	INNER JOIN ORDERS O
		ON C.ORDER_ID = O.ORDER_ID;

<img width=600px src="https://user-images.githubusercontent.com/46085656/189480909-743d1db4-06f7-49f3-9d53-63132505b06b.png">

## Left (Outer) Join
	-- Left join
	SELECT
		C.*,
		O.*
	 FROM CUSTOMERS C
	 LEFT JOIN ORDERS O
		ON C.ORDER_ID = O.ORDER_ID;

<img width=600px src="https://user-images.githubusercontent.com/46085656/189481838-0c8c59e4-4494-4432-a251-9ec08d3afc7a.png">

## Right (Outer) Join
	-- Right join
	SELECT
		C.*,
		O.*
	FROM CUSTOMERS C
	RIGHT JOIN ORDERS O
		ON C.ORDER_ID = O.ORDER_ID;

<img width=700px src="https://user-images.githubusercontent.com/46085656/189482070-324adad0-82f1-4965-be23-50b514b51f5e.png">

Note that `LEFT JOIN` is preferred method over `RIGHT JOIN` for better readability.

## Full (Outer) Join
	-- Full join
	SELECT
		C.*,
		O.*
	FROM CUSTOMERS C
	FULL JOIN ORDERS O
		ON C.ORDER_ID = O.ORDER_ID;

<img width=700px src="https://user-images.githubusercontent.com/46085656/189482094-e3a853e0-1e0b-4d20-9bad-f7a95631beb9.png">

## Anti-Join
	-- Left anti-join
	SELECT
		C.*,
		O.*
	FROM CUSTOMERS C
	LEFT JOIN ORDERS O
		ON C.ORDER_ID = O.ORDER_ID
	WHERE O.ORDER_ID IS NULL;
	
<img width=600px src="https://user-images.githubusercontent.com/46085656/189481979-5ae8525e-5a31-4902-9d7a-99bb2d16d9d5.png">

## Cross Join
	-- Cross join
	SELECT
		C.*,
		O.*
	FROM CUSTOMERS C
	CROSS JOIN ORDERS O
	
<img width=600px src="https://user-images.githubusercontent.com/46085656/189482228-fc05e9e1-1989-486b-9075-f0044b335a70.png">
<img width=600px src="https://user-images.githubusercontent.com/46085656/189482234-ddbc245b-873c-4165-810b-2f60ae46732a.png">
