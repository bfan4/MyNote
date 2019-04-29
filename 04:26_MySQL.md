04/26/2019  

#MYSQL数据库的索引、视图、触发器、游标和存储过程

 

（1）索引（index）...1

（2）视图（view）...2

（3）触发器（trigger）...6

（4）游标（cursor）...8

（5）事务(Transaction)10

（6）存储过程（Stored Procedure）...12


#SQL
-----------------------------------
https://www.generatedata.com
-----------------------------------

DDL ----- create tables, views, sp, triggers, cursors, index

DML ----- Insert, update & delete

select Query


启动MY SQL： /usr/local/mysql/bin/mysql -u root -p
创建数据库：  create database XXXXXXX
查看：desc XXXXXXX




JDBC Drivers
-----------------------------------
Oracle - OJDBC
MySQL  - mysql-connector
PostgreSQL - postgre


java.sql
----------
```java
loading a Driver 
		- Class.forName()

DriverManager
		- Connection genConnection(String url, String username, String password)

Connection
		- Statement -> createStatement()
		- PreparedStatement -> prepareStatement()
		- CallableStatement -> prepareCall()

	- Statement
		- (boolean) execute() - DDL
		- (int) executeUpdate() - DML
		- (ResultSet) executeQuery() - Select 

ResultSet
		- boolean next()
		- int genInt(int / String)
		- String getString(int / String)
		- date getDate(int / String)
		- Object getObject(int / String)
		- ResultSetMetaData getMetaData()

ResultSetMetaData
		- getColumnCount()
		- getColumnName(int)
		- getColumnType(int)
```




```sql
create table employee(
	id int primary key auto_increment,
	name varchar(30),
	age int,
	salary double,
	zipcode varchar(10)
);

insert into employee (name, age, salary, zipcode) values ("Tom", 25, 100000, "07087");
insert into employee (name, age, salary, zipcode) values ("Alice", 24, 104000, "07087");
insert into employee (name, age, salary, zipcode) values ("Bob", 25, 100060, "07087");
insert into employee (name, age, salary, zipcode) values ("Charly", 26, 120000, "07087");
```
































