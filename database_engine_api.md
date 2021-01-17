# Database Engine & API

## Database Engine
Database engine (storage engine) is the underlying software component that a database management system (DBMS) uses to create, read, update and delete (CRUD) data from a database. 

## API
Most database management systems come with their own GUI to interact with the database engines. However, database engines may be installed separately. And an API such as 
Open Database Connectivity (ODBC) or Object Linking and Embedding, Database (OLE DB) can be used to access the database engine. An application such as Python can access the 
database engine via the API.

### Open Database Connectivity (ODBC)
Developed by Microsoft, ODBC creates an interface called Driver to connect an application to a DBMS. The ODBC allows an application to access multiple DBMSs through drivers. 
ODBC is contrained to relational data stores and SQL. 

### Object Linking and Embedding, Database (OLE DB)
OLE DB is built upon ODBC by Microsoft to support all forms of data stores (relational & non-relational). While ODBC is a procedural based specification, OLE DB is 
a component based specification. 
