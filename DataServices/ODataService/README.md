# SimpleODataService.dbs

This example demonstrates how an RDBMS can be exposed as an OData service. When OData is enabled, you do not need to manually define CRUD operations. Therefore, OData services are an easy way to enable CRUD operations for a data service.

## Prerequisites
1. Create a MySQL database named CompanyAccounts

```sql
CREATE DATABASE CompanyAccounts;
```
2. Create a table in the CompanyAccounts database as follows.
```sql
CREATE TABLE ACCOUNT(
    AccountID int NOT NULL,
    Branch varchar(255) NOT NULL, 
    AccountNumber varchar(255),
    AccountType ENUM('CURRENT', 'SAVINGS') NOT NULL,
    Balance FLOAT,
    ModifiedDate DATE,
    PRIMARY KEY (AccountID)); 
```
3. Enter the following data into the table:
```sql
INSERT INTO ACCOUNT VALUES (1,"AOB","A00012","CURRENT",231221,'2010-12-02');
INSERT INTO ACCOUNT VALUES (2,"AOC","A00013","SAVINGS",112345,'2011-11-02');
INSERT INTO ACCOUNT VALUES (3,"AOD","A00014","SAVINGS",431234,'2020-01-05');
INSERT INTO ACCOUNT VALUES (4,"AOE","A00015","CURRENT",989028,'2022-12-22'); 
```
4. Navigate to the <MI-Home>/repository/deployment/server/synapse-configs/default/dataservices directory and create a new file named SimpleODataService.xml. Add the following content to the file.

## Invoke the service

To get the service document:
```
curl -X GET -H 'Accept: application/json' http://localhost:8290/odata/odata_service/Datasource
```

To get the metadata of the service:
```
curl -X GET -H 'Accept: application/xml' http://localhost:8290/odata/odata_service/Datasource/$metadata
```

To read details from the ACCOUNT table:
```
curl -X GET -H 'Accept: application/xml' http://localhost:8290/odata/odata_service/Datasource/ACCOUNT
```