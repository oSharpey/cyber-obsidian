# Lecture 2 - Database Management Systems

## What is a database
- A collection of interrealted the data
	- What the data is and how its connected
- A database can be of any size and varying in complexity
- A visual representation of a database which defines how data is organised within a database, inclusive of: 
	- logical constraints such as, table names, fields, data types, and th e relationships between these entities
	- Database Scheme - blueprint of how the data will be stored

## Database Management System
- Programs that enable users to create manipulate and extract data from databases
- DBMS perform various types of operations such as insert, update and delete data records

### Functions of a DBMS
- **Query Processing and Optimisation**
	- Processing - Converts queries into a low level that is then optimised
	- Optimisation - Perfect strategy for a query execution
- **Data Definition Language** - Allows users to vreate database and alter structures
- **Data Manipulation Language** - Allows users to manipulate data in the database
- **Concurrency Control** - Schedules multiple transactions in a database and stops them being executed at the same time
- **Recovery** - Ensure if database failed, the data remains consitent 
- **Security Control** - 