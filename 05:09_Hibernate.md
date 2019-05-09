05/09

##Hibernate  

###Step1.
先建立hibernate.cfg.xml配置文件：  


```xml
<?xml version='1.0' encoding='utf-8'?>

<!DOCTYPE hibernate-configuration PUBLIC
       "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd"> 
<hibernate-configuration>
 
<session-factory>
        <!-- Database connection settings -->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="connection.url">jdbc:mysql://localhost:3306/test?characterEncoding=UTF-8</property>
        <property name="connection.username">root</property>
        <property name="connection.password">admin</property>
        <!-- SQL dialect -->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>
        <property name="current_session_context_class">thread</property>
        <property name="show_sql">true</property>
        <property name="hbm2ddl.auto">update</property>
        
        <!-- List of XML mapping files --
        <mapping class="com.demo.model.Book" />
    </session-factory>
 
</hibernate-configuration>
```
###Step.2  
然后建立hibernate的mapping：（这里以建立一个```Book.class```的mapping为例）  

```html
<?xml version='1.0' encoding='utf-8'?>

<!DOCTYPE hibernate-configuration PUBLIC
       "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd"> 

<hibernate-mapping>

	<class name = "com.demo.model.Book" table = "Books">
	
		<id name = "id" column = "book_id">
			<generator class="increment" />
		</id>
		
		<property name = "title" column = "book_title"/>
		<property name = "author" column = "book_author"/>
		<property name = "price" column = "book_price"/>
	
	</class>
</hibernate-mapping>	
		
```
###Step3.  
在pem.xml中导入Maven的hibernate dependency，sql dependency:  

```xml
<dependencies> 
  
    <dependency>
    	<groupId>org.hibernate</groupId>
    	<artifactId>hibernate-core</artifactId>
    	<version>4.3.2.Final</version>
	</dependency>
	
	<dependency>
    	<groupId>mysql</groupId>
    	<artifactId>mysql-connector-java</artifactId>
    	<version>8.0.15</version>
 	</dependency>
 	
    <dependency>
      	<groupId>junit</groupId>
      	<artifactId>junit</artifactId>
      	<version>3.8.1</version>
      	<scope>test</scope>
    </dependency>
    
  </dependencies>
```

###Step4.  
写Class的CRUD方法：  

* 创建会话工厂```SessionFactory``` （有点像JDBC里的Connectiont） 
* 从```SessionFactory```中创建```Session session = factory.openSession();```
* 创建```Transaction t = session.beginTransaction();```
* 使用session进行与数据库的通信
* 结束之后```t.commit(); session.close();```关闭Session

```java
package com.demo.main;


import java.util.List;

import org.hibernate.Query;
import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

import com.demo.model.Book;

public class CreateBooks {
    SessionFactory factory;
    
    CreateBooks(){
        try {
            factory = new Configuration().configure("hibernate.cfg.xml").buildSessionFactory();
            
        }catch( Exception e) {
            e.printStackTrace();
        }
    }
    
    void insertBook() {
        try {
            Book book = new Book("java", "Herbert", 15.99);
            Book book2 = new Book("C++", "Burec", 19.99);
            Session session = factory.openSession();
            Transaction t = session.beginTransaction();
            session.persist(book);
            session.persist(book2);
            t.commit();
            session.close();
        }catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    List<Book> viewAll(){
        
        Session session = factory.openSession();
        Query query = session.createQuery("from Book ");
        List<Book> list = query.list();
        
        
        return list;
    }
    
    public static void main(String[] args) {
        
        //new CreateBooks().insertBook();
        
        List<Book> list = new CreateBooks().viewAll();
        
        for (Book book : list) {
            System.out.println(book);
        }
        
        }

}

```

##使用Hibernate建表：
使用注解建表的方法：  

* 注意：使用注解方法的时候，配置文件中的```<mapping resource="com/how2java/pojo/Product.hbm.xml" />```应该改成``` <mapping class="com.how2java.pojo.Product" />``` 
* 更多注解用法，参考[注解手册](http://how2j.cn/k/hibernate/hibernate-annotation-manual/1051.html)


```java
package com.demo.model;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table( name="books")
public class Book {
    
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private int id;
    
    @Column(length=30)
    private String title;
    
    @Column
    private String author;
    
    @Column
    private double price;
    
    @OneToOne(cascade = CascadeType.ALL, mappedBy = "book", fetch = FetchType.LAZY)
    private BookDetail bookDetail;
    /*
    同时你也需要在BookDetail里加入这段代码：
    
    @OneToOne(fetch=FetchType.LAZY)
    @JoinColumn(name="BOOK_ID")
    private Book book;  
    */
    public Book() {
        
    }
}

```
XML配置方式：    

* 优：容易编辑，配置比较集中，方便修改，在大业务量的系统里面，通过xml配置会方便后人理解整个系统的架构，修改之后直接重启应用即可   
* 缺：比较繁琐，配置形态丑陋, 配置文件过多的时候难以管理 

注解方式： 
  
* 优：方便，简洁，配置信息和 Java 代码放在一起，有助于增强程序的内聚性。   
* 缺：分散到各个class文件中，所以不宜维护, 修改之后你需要重新打包，发布，重启应用。 












