# Lecture 2 - Database Management Systems

## What is a database
- A collection of inter-realted the data
	- What the data is and how its connected
- A database can be of any size and varying in complexity
- A visual representation of a database which defines how data is organised within a database, inclusive of: 
	- logical constraints such as, table names, fields, data types, and the relationships between these entities
	- Database Scheme - blueprint of how the data will be stored

## Database Management System
- Programs that enable users to create manipulate and extract data from databases
- DBMS perform various types of operations such as insert, update and delete data records

### Functions of a DBMS
- **Query Processing and Optimisation**
	- Processing - Converts queries into a low level that is then optimised
	- Optimisation - Perfect strategy for a query execution
- **Data Definition Language** - Allows users to create database and alter structures
- **Data Manipulation Language** - Allows users to manipulate data in the database
- **Concurrency Control** - Schedules multiple transactions in a database and stops them being executed at the same time
- **Recovery** - Ensure if database failed, the data remains consistent 
- **Security Control** - Prevent unauthorised access to data
- **Data Integrity** - enforces integrity constraints whenever a change is made to the database to ensure the database is consistent and correct.

### Applications
- Banking
- Airlines
- Universities
- Online Retailers

### Advantages
- **Data Consistency** 
	- DBMS makes sure consistency is maintained and shares throughout the organisation
- **Effective Data Integration**
	- Provides integrated picture how processes in one segment of the organisation affect other segments.
- **Data Sharing and Security**
	- Enables you to share the data so its easy to access and secured
- **Reducing data redundancy**
	- Avoids duplicate files 
	- Any changes in a databases reflected immediately. 
	- Hence there is no chance of encountering duplicate data.

### Relational database management systems
- Based on relational models
- Tables in relational databases are linked to each other
- Allows sorting, storing, organising and managing of the data more efficiently.
- Minimises repeated and redundant data

## Data Models
- The way the data is going to be stored
- Described how data is stored, accessed and updated in a DBMS
- Types:
	- Entity Relationship Diagram
	- Normalisation 

### Advantages
- Ensure data is represented accurately 
- Data redundancy is minimised
- Security is not compromised
### Disadvantages
- Difficult to understand
- Costs can be high