# RDBMSDataService


## Prerequisites
1. Create a MySQL database named Employees

```sql
CREATE DATABASE Employees;
```
2. Create a table in the CompanyAccounts database as follows.
```sql
USE Employees;

CREATE TABLE Employees (EmployeeNumber int(11) NOT NULL, FirstName varchar(255) NOT NULL, LastName varchar(255) DEFAULT NULL, Email varchar(255) DEFAULT NULL, Salary varchar(255));
```
3. Enter the following data into the table:
```sql
INSERT INTO Employees (EmployeeNumber, FirstName, LastName, Email, Salary) VALUES (1, "John", "Doe", "john.doe@example.com", "50000");
INSERT INTO Employees (EmployeeNumber, FirstName, LastName, Email, Salary) VALUES (2, "Jane", "Doe", "jane.doe@example.com", "60000");
INSERT INTO Employees (EmployeeNumber, FirstName, LastName, Email, Salary) VALUES (3, "Bob", "Smith", "bob.smith@example.com", "70000");
```
4. Navigate to the <MI-Home>/repository/deployment/server/dataservices directory and create a new file named RDBMSDataService.dbs. Add the following content to the file.
```xml
<data enableBatchRequests="true" name="RDBMSDataService" serviceGroup="" serviceNamespace="">
    <description/>
    <query id="GetEmployeeDetails" useConfig="Datasource">
        <sql>select EmployeeNumber, FirstName, LastName, Email, Salary from Employees where EmployeeNumber=:EmployeeNumber</sql>
        <param name="EmployeeNumber" paramType="SCALAR" sqlType="STRING"/>
        <result element="Employees" rowName="Employee">
            <element column="EmployeeNumber" name="EmployeeNumber" xsdType="xs:string"/>
            <element column="FirstName" name="FirstName" xsdType="xs:string"/>
            <element column="LastName" name="LastName" xsdType="xs:string"/>
            <element column="Email" name="Email" xsdType="xs:string"/>
            <element column="Salary" name="Salary" xsdType="xs:string"/>
        </result>
    </query>
  <config id="Datasource">
    <property name="driverClassName">com.mysql.cj.jdbc.Driver</property>
    <property name="url">jdbc:mysql://localhost:3306/Employees</property>
    <property name="username">root</property>
    <property name="password">root</property>
  </config>
    <query id="AddEmployeeDetails" useConfig="Datasource">
        <sql>insert into Employees (EmployeeNumber, FirstName, LastName, Email, Salary) values(:EmployeeNumber,:FirstName,:LastName,:Email,:Salary)</sql>
        <param name="EmployeeNumber" paramType="SCALAR" sqlType="STRING"/>
        <param name="FirstName" paramType="SCALAR" sqlType="STRING"/>
        <param name="LastName" paramType="SCALAR" sqlType="STRING"/>
        <param name="Email" paramType="SCALAR" sqlType="STRING"/>
        <param name="Salary" paramType="SCALAR" sqlType="STRING"/>
    </query>
    <query id="UpdateEmployeeDetails" useConfig="Datasource">
        <param name="EmployeeNumber" paramType="SCALAR" sqlType="STRING"/>
        <sql>update Employees set FirstName=:FirstName, LastName=:LastName, Email=:Email, Salary=:Salary where EmployeeNumber=:EmployeeNumber</sql>
        <param name="FirstName" paramType="SCALAR" sqlType="STRING"/>
        <param name="LastName" paramType="SCALAR" sqlType="STRING"/>
        <param name="Email" paramType="SCALAR" sqlType="STRING"/>
        <param name="Salary" paramType="SCALAR" sqlType="STRING"/>
    </query>
    <resource method="GET" path="Employee/{EmployeeNumber}">
        <call-query href="GetEmployeeDetails">
            <with-param name="EmployeeNumber" query-param="EmployeeNumber"/>
        </call-query>
    </resource>
    <resource method="POST" path="Employee">
        <call-query href="AddEmployeeDetails">
            <with-param name="EmployeeNumber" query-param="EmployeeNumber"/>
            <with-param name="FirstName" query-param="FirstName"/>
            <with-param name="LastName" query-param="LastName"/>
            <with-param name="Email" query-param="Email"/>
            <with-param name="Salary" query-param="Salary"/>
        </call-query>
    </resource>
    <resource method="PUT" path="Employee">
        <call-query href="UpdateEmployeeDetails">
            <with-param name="EmployeeNumber" query-param="EmployeeNumber"/>
            <with-param name="FirstName" query-param="FirstName"/>
            <with-param name="LastName" query-param="LastName"/>
            <with-param name="Email" query-param="Email"/>
            <with-param name="Salary" query-param="Salary"/>
        </call-query>
    </resource>
</data>
```

## Invoke the service

To get the Employee with EmployeeNumber 3:
XML response
```
curl -X GET http://localhost:8290/services/RDBMSDataService/Employee/3
```
JSON response
```
curl --header 'Accept:application/json' -X GET http://localhost:8290/services/RDBMSDataService/Employee/3
```

To add an employee:
```
curl -X POST -H 'Accept: application/xml'  -H 'Content-Type: application/xml' \
--data '<_postemployee>
    <EmployeeNumber>4</EmployeeNumber>
    <FirstName>Will</FirstName>
    <LastName>Smith</LastName>
    <Email>will@google.com</Email>
    <Salary>15500.0</Salary>
</_postemployee>' http://localhost:8290/services/RDBMSDataService/employee
```

To update an employee:
```
curl -X PUT -H 'Accept: application/xml'  -H 'Content-Type: application/xml' \
--data '<_putemployee>
    <EmployeeNumber>3</EmployeeNumber>
    <LastName>Smith</LastName>
    <FirstName>Will</FirstName>
    <Email>will@google.com</Email>
    <Salary>30000.0</Salary>
</_putemployee>' http://localhost:8290/services/RDBMSDataService/employee
```

To get the swagger definition of the service:
```
http://localhost:8290/services/RDBMSDataService?swagger.json

http://localhost:8290/services/RDBMSDataService?swagger.yaml
```
