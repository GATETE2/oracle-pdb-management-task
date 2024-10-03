  


 ### Oracle Pluggable Database (PDB) Creation and Deletion Task
 ----------------------------------------------------------------------------------------

This document outlines the process of creating and deleting a Pluggable Database (PDB) in Oracle Database as part of an assignment for managing databases in Oracle RDBMS. The steps include creating the PDB, switching between containers, and finally deleting the PDB after performing the necessary operations. Additionally, it covers the configuration and use of Oracle Enterprise Manager to monitor and manage database environments, providing a visual interface to validate and track the PDB lifecycle.

## Overview of the Task
This task demonstrates the fundamental operations for working with pluggable databases, including:

### Creating a Pluggable Database (PDB): A new PDB named plsql_class2024db was created using SQL commands.
## Managing the PDB: This includes switching to the newly created PDB and checking its status using SQL queries.
## Deleting the PDB: A temporary PDB named ga_to_delete_pdb was created, then deleted after being unplugged, ensuring proper removal from the container database.

## Oracle Database Enterprise Manager Configuration
---------------------------------------------------------------

Oracle Database Enterprise Manager (OEM) was used to visually confirm and manage the PDBs. The OEM dashboard provides real-time status, performance metrics, and management capabilities for both the container database and its PDBs. Screenshots have been captured to show:

Creating and Opening a PDB in Oracle Enterprise Manager
Monitoring PDB Status and Operations
Deleting and Unplugging PDBs via OEM

## SQL Queries and Explanations
----------------------------------------------------------------------

1. Check Existing Pluggable Databases

SELECT name, open_mode FROM v$pdbs;
This query retrieves the names and open modes of all existing pluggable databases in the container database, allowing us to see the current state of the PDBs.

2. Create Pluggable Database plsql_class2024db

 ```sql
CREATE PLUGGABLE DATABASE plsql_class2024db
ADMIN USER ga_plsqlauca IDENTIFIED BY 'admin'
ROLES = (DBA)
FILE_NAME_CONVERT = ('/u01/app/oracle/oradata/orcl/pdbseed/', '/u01/app/oracle/oradata/orcl/plsql_class2024db/');
```
This command creates a new PDB named plsql_class2024db with an administrative user ga_plsqlauca. The password is set to 'admin', and the database is assigned the DBA role. The FILE_NAME_CONVERT clause specifies the path for the new PDB data files.

3. Open the PDB

ALTER PLUGGABLE DATABASE
```sql
 plsql_class2024db OPEN;
 ```

This command opens the newly created PDB, making it accessible for operations.

4. Switch to plsql_class2024db PDB
```sql
ALTER SESSION SET CONTAINER = plsql_class2024db;
```
This command switches the current session to the plsql_class2024db PDB, allowing for operations to be performed within this database.

5. Check if plsql_class2024db is Open
```sql
SELECT name, open_mode FROM v$pdbs WHERE name = 'PLSQL_CLASS2024DB';
```
This query checks the status of the plsql_class2024db PDB to confirm it is open.

 ## creating and Deleting pluggable database  ga_to_delete_pdb
 ----------------------------------------------------------------

1. Create Pluggable Database ga_to_delete_pdb
```sql
CREATE PLUGGABLE DATABASE ga_to_delete_pdb
ADMIN USER ga_delete_admin IDENTIFIED BY 'admin'
ROLES = (DBA)
FILE_NAME_CONVERT = ('/u01/app/oracle/oradata/orcl/pdbseed/', '/u01/app/oracle/oradata/orcl/ga_to_delete_pdb/');
```
This command creates a temporary PDB named ga_to_delete_pdb with an administrative user ga_delete_admin, password 'admin', and the DBA role.


2. Close ga_to_delete_pdb Before Deletion
```sql
ALTER PLUGGABLE DATABASE ga_to_delete_pdb CLOSE IMMEDIATE;
```
This command closes the ga_to_delete_pdb, preparing it for deletion.


3. Drop ga_to_delete_pdb Including Datafiles
```sql
DROP PLUGGABLE DATABASE ga_to_delete_pdb INCLUDING DATAFILES;
```
This command permanently deletes the ga_to_delete_pdb, removing it from the container database along with its associated data files.
