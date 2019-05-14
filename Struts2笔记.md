#Struts2知识点  
##1. Struts工作流程  

1	客户端（浏览器）提交HttpServletRequest请求，例如insert.action  
2	请求在服务器中通过一个个过滤器，最终会经过Struts2的核心过滤器  
3	核心过滤器判断请求是否符合web.xml中配置的url-pattern，如果是则进入Struts框架处理，如果不是，则放行由其他控制其处理  
4	进入Struts2后，通过配置文件struts.xml判断由哪个Action对这个请求进行处理,并且找到处理这个Action的类  
5	如果这个请求被拦截器拦截，则6会在拦截器中的操作。  
6	Action类处理完了请求之后，返回一个代表结果视图的resultString  
7	通过配置文件判断返回的resultString代表哪个视图，再进行跳转或输出  

##2. jar包准备    
准备jar包，然后copy到web项目的WebRoot/WEB-INF/lib下，maven项目除外。我是用的是IDE是Eclipse，所以默认的根目录是WebContent，无异。  

##3. 配置核心过滤器  
Struts2的核心过滤器为StrutsPrepareAndExecuteFilter，我们只要在web.xml中配置一下这个过滤器就好了。过滤器全名为：org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter   

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <display-name>S2Review</display-name>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
  </welcome-file-list>
  <filter>
    <filter-name>struts2</filter-name>
    <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>struts2</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```  

##4. 核心配置文件   

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
    "http://struts.apache.org/dtds/struts-2.0.dtd">
<struts>
    <!-- 引入其他配置文件 -->
    <include file="strutsconfig/global.xml"/>
    <!-- 设置struts2属性(此处为开启开发模式，spring整合) -->
    <constant name="struts.devMode" value="true" />
    <constant name="struts.objectFactory" value="spring" />

    <!-- 分模块开发 -->
    <package name="member" namespace="/member" extends="dgcrm-global">
        <!-- 全局结果视图 -->
        <global-results>
            <result name="error">/error.jsp</result>
            <result name="exception">/exception/exception.jsp</result>
            <result name="err">/exception/error.jsp</result>
        </global-results>
        <!-- 配置action和对应的action类 因为此处与spring整合，所以class写beanname-->
        <action name="memberList" method="memberList"
            class="memberAction">
            <result>../jsp/allBusi/member/memberListData.jsp</result>
        </action>
        <action name="getMemberJsp">
            <!-- 只配置一个name默认为SUCCESS -->
            <result>../jsp/allBusi/member/memberList.jsp</result>
        </action>
        <!-- 转发到另外一个aciton 
               dispatcher:默认结果类型。请求转发到一个页面。
               redirect：请求重定向到一个页面。
               chain:请求转发到另一个动作。
               redirectAction：重定向到另外一个动作
               stream：下载用的
               plainText：以纯文本的形式展现内容

        -->
        <action name="ch">
            <result type="chain">getMemberJsp</result>
        </acction>

    </package>
</struts>
``` 

## 5. Action是干什么的？  
无论你是使用Struts2还是SpringMVC，你都必须知道他们都是基于MVC架构的，而Struts2中的Action和SpringMVC中的Controller都属于架构中的C，也就是Control层，负责客户端与服务器的数据交换，流程控制。比如页面表单提交了用户名密码，Control层的作用就是接收用户名密码，并且调用相关服务或者类验证准确性，最后再回应客户端需要显示什么View结果。   

Action在Struts2中至关重要，让数据传到后台，就已经可以把参数当做本地变量处理了。Action实质上是一个Servlet，通过框架的内部实现和功能增强，让数据不再需要通过request.getParameter()来获取，只需要将参数作为成员变量定义在Action类中，框架就会帮我们注入参数值，看起来就像是在调取本地变量。  
###Action的写法
####要求  
 * 继承ActionSupport
 * 方法为public
 * 方法无参数
 * 方法返回值为String类型   
 
####写法
MyAction.java   
 
```java
package action;

import com.opensymphony.xwork2.ActionSupport;

public class MyAction extends ActionSupport {
    private static final long serialVersionUID = 1L;
    //参数直接被框架注入到Action类的成员变量中
    private String name;
    private int age;

    //action方法，在配置文件中配置映射
    public String hi(){
        System.out.println("run---> hi,"+name+","+age);
        return SUCCESS;
    }

    public String hello(){
        System.out.println("run---> hello,"+name+","+age);
        return SUCCESS;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}
```

映射关系配置文件```struts.xml```   

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
    "http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <!-- namespace要用"/" action不用-->
    <package name="myaction" extends="struts-default" namespace="/myaction">
        <action name="*" class="action.MyAction" method="{1}">
            <result name="success">/success.jsp</result>
        </action>
        <!-- 也可以写成↓↓↓
        <action name="hi" class="action.MyAction" method="hi">
            <result name="success">/success.jsp</result>
        </action>
        -->

    </package>
</struts>
```
####在Action中怎么访问一些常用的实例  
在Action中，我们要取得像pageContext、request、response、servletContext该写什么语句呢？  

```java
//通过ServletActionContext类的静态方法取得。
ServletActionContext.getPageContext();
ServletActionContext.getRequest();
ServletActionContext.getResponse();
ServletActionContext.getServletContext();
```


##6. Struts2传递参数
在远古时期，也就是我们使用Servlet + jsp 的时期，提交一个表单，传递的是一个个的字符串。尽管表单页面我们能设计得非常多样，但是到了传递的时候，最终还是将其化作字符串传递到服务端。

在Struts2中，框架替我们做了很多便利的事，比如，我们可以通过表单组件的name，来决定传递的数据在Action中被解析成**普通参数**、**对象**、**List**、还是**Map**。   
### 传递形式
在复习Action的时候说过，Action中要获取页面传递过来的参数，只需要在Action中定义成员变量并且提供设置器(Setter)和获取器(Getter)，Struts2就会帮我们把数据封装进去。传递对象也一样，页面中填的依旧是字符串，只不过到了Action之后框架会根据参数的名称name来判读要“组装”成什么样的参数。  
###写法  
下面就以Person，List<Person>, Map<String, Person>接收参数为例：  

Person.java   
 
```java
package action;

import java.io.Serializable;

public class Person implements Serializable{
    @Override
    public String toString() {
        return "Person [name=" + name + ", age=" + age + "]";
    }
    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```
ArgsTestAction.java  

```
package action;

import java.util.List;
import java.util.Map;

import com.opensymphony.xwork2.ActionSupport;

public class ArgsTestAction extends ActionSupport {
    //装参数的变量们
    private Person person;
    private List<Person> personList;
    private Map<String,Person> personMap;

    public String comeOn(){
        System.out.println(person.toString());

        System.out.println("\nlist of person:");
        for(Person p : personList){
            System.out.println(p);
        }

        System.out.println("\nmap of person:");
        for(String key : personMap.keySet()){
            System.out.println(personMap.get(key));
        }
        return SUCCESS;
    }
    public Person getPerson() {
        return person;
    }

    public void setPerson(Person person) {
        this.person = person;
    }

    public List<Person> getPersonList() {
        return personList;
    }

    public void setPersonList(List<Person> personList) {
        this.personList = personList;
    }

    public Map<String, Person> getPersonMap() {
        return personMap;
    }

    public void setPersonMap(Map<String, Person> personMap) {
        this.personMap = personMap;
    }
}
```  
配置文件struts.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
    "http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <!-- namespace要用"/" action不用-->
    <package name="test" extends="struts-default" namespace="/test">
        <action name="comeOn" class="action.ArgsTestAction" method="comeOn">
            <result name="success">/success.jsp</result>
        </action>

    </package>
</struts>
```  
输入页面input.jsp  

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <form action="${pageContext.request.contextPath }/test/comeOn" method="post">
        <!-- Person对象传递，使用"Action定义的对象名.对象属性"形式获取 -->
        用户1 >>>姓名：<input type="text" name="person.name">年龄：<input type="text" name="person.age"><br>

        <!-- List对象传递，使用"Action定义的List名.[坐标].对象属性"形式获取 -->
        用户2 >>>姓名：<input type="text" name="personList[0].name">年龄：<input type="text" name="personList[0].age"><br>
        用户3 >>>姓名：<input type="text" name="personList[1].name">年龄：<input type="text" name="personList[1].age"><br>

        <!-- Map对象传递，使用"Action定义的Map名.[key].对象属性"形式获取 -->
        用户4 >>>姓名：<input type="text" name="personMap['no1'].name">年龄：<input type="text" name="personMap['no1'].age"><br>
        用户5 >>>姓名：<input type="text" name="personMap['no2'].name">年龄：<input type="text" name="personMap['no2'].age"><br>
        <input type="submit">
    </form>
</body>
</html>
```  

##7. 拦截器的工作流程以及如何实现
###工作流程  

顾名思义，拦截器就是在请求前后对请求进行拦截，抽象的说法就是拦截器将Action抱在了自己怀里，别人想要访问就必须经过拦截器这道门，离开也要从这道门经过。抽象的图如下 ：

 
![avatar](https://img-blog.csdn.net/20170730130847870?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjQ0NDg4OTk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

###应用场景  
在Web应用中，很多地方都用到了拦截器，例如权限验证，编码转换，参数类型转换（Struts2对参数的封装就是通过拦截器实现的），日志输出等。  
###拦截器栈  
在Struts2中，拦截器像栈一样将action方法压在栈的最底层，要访问到最底层的action方法，就必须从栈顶逐级访问拦截器，到达最底层执行完方法后，又依次从栈底按照顺序逐级退出。在struts-default中预先定义了很多默认的拦截器，在我们编写自定义拦截器的时候最好在其之前声明默认的拦截器栈，以防参数获取出错或其它错误。以下为默认拦截器栈defaultStack中的拦截器们：  

```xml
<interceptor-stack name="defaultStack">
    <interceptor-ref name="exception"/>
    <interceptor-ref name="alias"/>
    <interceptor-ref name="servletConfig"/>
    <interceptor-ref name="i18n"/>
    <interceptor-ref name="prepare"/>
    <interceptor-ref name="chain"/>
    <interceptor-ref name="scopedModelDriven"/>
    <interceptor-ref name="modelDriven"/>
    <interceptor-ref name="fileUpload"/>
    <interceptor-ref name="checkbox"/>
    <interceptor-ref name="multiselect"/>
    <interceptor-ref name="staticParams"/>
    <interceptor-ref name="actionMappingParams"/>
    <interceptor-ref name="params">
        <param name="excludeParams">dojo\..*,^struts\..*,^session\..*,^request\..*,^application\..*,^servlet(Request|Response)\..*,parameters\...*</param>
    </interceptor-ref>
    <interceptor-ref name="conversionError"/>
    <interceptor-ref name="validation">
        <param name="excludeMethods">input,back,cancel,browse</param>
    </interceptor-ref>
    <interceptor-ref name="workflow">
        <param name="excludeMethods">input,back,cancel,browse</param>
    </interceptor-ref>
    <interceptor-ref name="debugging"/>
</interceptor-stack>
```

###具体定义方式
####要求
 * 实现Interceptor接口， 或者继承AbstractInterceptor类。
 * 实现包含intercept方法，这个方法为每次请求访问时拦截器执行的方法他有一个ActionInvocation类型的参数，我们可以通过这个方法轻易地获取到请求访问的Action。方法签名为：```public String intercept(ActionInvocation actionInvocation) throws Exception```
 * 根据需要选择覆盖以下方法： 
 
>```init()```在服务器加载应用时，进行的一些初始化操作  
>```destory()```在拦截器销毁时，进行的清理操作  
>
###简单范例  
下面做一个简单示例，模拟用户注册事件，在密码传输到Action之前，将其使用MD5加密，其中MD5加密使用的是自己实现的一个工具类。此处没有重写init()和destory()方法。**注意：配置文件中一定要配置默认的拦截器栈，因为一旦加入了自定义拦截器，默认拦截器栈就会失效，在加入时要注意将其放在自定义拦截器之前。**

InterceptorAction.java

```java
package action;

import util.MD5Code;

import com.opensymphony.xwork2.ActionSupport;

public class InterceptorAction extends ActionSupport{
    private String account;
    private String password;

    public String regist(){
        //模拟存进数据库
        System.out.println("把用户：" + account +"存进了数据库");
        System.out.println("密码为：" + password);
        return SUCCESS;
    }

    public String getAccount() {
        return account;
    }
    public void setAccount(String account) {
        this.account = account;
    }
    public String getPassword() {
        return password;
    }
    public void setPassword(String password) {
        this.password = password;
    }

}
```  

自定一拦截器： RegistInterceptor.java

```java
package interceptor;

import util.MD5Code;
import action.InterceptorAction;

import com.opensymphony.xwork2.ActionInvocation;
import com.opensymphony.xwork2.interceptor.AbstractInterceptor;

public class RegistInterceptor extends AbstractInterceptor{

    public String intercept(ActionInvocation actionInvocation) throws Exception {
        System.out.println(">>>到达action前进入拦截器...");
        InterceptorAction action = (InterceptorAction)actionInvocation.getAction();
        String password = (action.getPassword());
        //通过工具类将加密后的密码设置成参数
        action.setPassword(MD5Code.MD5(password));
        //放行，之后将进入action
        actionInvocation.invoke();
        System.out.println(">>>执行完action进入拦截器...");
        return null;
    }
}
```
配置文件：struts.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
    "-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
    "http://struts.apache.org/dtds/struts-2.3.dtd">
<struts>
    <!-- namespace要用"/" action不用-->
    <package name="interceptorTest" extends="struts-default" namespace="/itest">
        <interceptors>
            <interceptor name="registInterceptor" class="interceptor.RegistInterceptor"></interceptor>
        </interceptors>
        <action name="regist" class="action.InterceptorAction" method="regist">

            <result name="success">/success.jsp</result>
            <!-- 要加上默认的拦截器，不然无法获取参数 -->
            <interceptor-ref name="defaultStack"/>
            <interceptor-ref name="registInterceptor"></interceptor-ref>
        </action>

    </package>
</struts>

```




---
---
*来源：CSDN   
原文：https://blog.csdn.net/qq_24448899/article/details/76380769   
版权声明：本文为博主原创文章，转载请附上博文链接！*  

























