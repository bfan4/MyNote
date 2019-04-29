04/29/2017  


```
docker pull xrdj6c/oracle-11g-xe
```
# How-To: Install and Use docker pull xrdj6c/oracle-11g-xe

Run with 22, 1521 and 8080 ports opened: docker run -d -p 49160:22 -p 1521:1521 -p 8080:8080 xrdj6c/oracle-11g-xe Connect database with following setting:

hostname: localhost port: 1521 sid: xe 
username: system password: oracle Password for SYS

Connect to Oracle Application Express web management console with following settings:

url: http://localhost:8080/apex workspace: INTERNAL user: ADMIN password: oracle Login by SSH

ssh root@localhost -p 49160 password: admin OR sudo docker exec -i -t bash

```
docker pull xrdj6c/oracle-11g-xe
docker run -d -p 49160:22 -p 1521:1521 -p 49162:8080 xrdj6c/oracle-11g-xe
docker exec -it <container-id> bash
```

/# sqlplus
>Username:system.  
>Password:oracle.    


# IN & OUT

```
create procedure sp1 as
	begin
  	update users set username='jsmith' where id= 10;
  	end;
  	/
```
####invoke with ```call```
---

```
create or replace procedure sp2 ( str IN string, uid IN number) 
is begin
update users set password = str where id = uid;
end;
/
``` 
####invoke with ```exec```
---
```
create of replace procedure sp3( n1 IN number, n2 IN number, n3 OUT number) as
begin
n3 := n1 * n2;
end;
/
```
####invoke with
```
set serveroutput ON;  //set the server ouput ON first
```
```
declare num number;
begin
sp3(15, 20, num);
dbms_output.put_line(num);
end;
/
```

#导入MVN jar.文件
```
mvn install:install-file -Dfile=/Users/brucevan/documents/ojdbc14.jar -DgroupId=com.marlabs.oracle -DartifactId=oracle-jdbc -Dversion=1.4 -Dpackaging=jar
```  





# Java WEB Technologies
## WEB 文件框架结构：
```
>Hello-Web
     > WebContent
     		> xxx.html
     		> xxx.jsp
     		> WEB-INF
     			> web.xml
     			> lib folder
     			> xxx.tld
     			> xxx.properties
     			> xxx.xml
     		> script folder
     			> xxx.js
     		> style folder
     			> xxx.css
     		> images folder
     			> xxx.jpeg, xxx.png
     	> src
     		> All xxx.java packages
     			> java classes
     				> POJO
     				> Controller
     				> BO
     				> Service
     				> DTO/DAO
     				> Utility
     				> Servlets
     		> xxx.xml
     		> xxx.propertyies
     	> pom.xml
```
## Server side:

- Servlets: 
A server side program which accepts HtTP request from a browser, processes it on the server side and sends back the response in the form of dynamic HTML web page is called as Servlet.
- JSP

```python
foo = (1,2,3,4,5)
```

















