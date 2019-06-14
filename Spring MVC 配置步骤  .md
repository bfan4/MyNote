##  Spring MVC 配置步骤  

###  Step 1: Configure Spring MVC Dispatcher Servlet  
in ```web.xml```

```xml
<servlet>
	<servlet-name>dispatcher</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring-mvc-demo-servlet.xml</param-value>
	</init-param>
	<load-on-startup>1</load-on-startup>
</servlet>

``` 

### Step 2: Set up URL mapping for Spring MVC Dispatcher Servlet  
in```web.xml```
```xml
<servlet-mapping>
	<servlet-name>dispatcher</servlet-name>
	<url-pattern>/<url-pattern>
</servlet-mapping>
```

###  Step 3: Add support for component scanning  
in```spring-mvc-demo-servlet.xml```

```xml
<context:component-scan base-package="com.xxxxxx.springdemo"/>
```

### Step 4: Add support for conversion, formatting and validation support  
in```spring-mvc-demo-servlet.xml```  

```xml
<mvc:annotation-driven/>
```

### Step 5: Define Spring MVC view resolver
in```spring-mvc-demo-servlet.xml```

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
	<property name="prefix" value="/WEB-INF/view/"/>
	<property name="suffix" value=".jsp"/>
</bean>
```















