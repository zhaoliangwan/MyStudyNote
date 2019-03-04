* [SpringMVC 学习笔记](#springmvc-学习笔记)
  * [前言](#前言)
  * [第1章 Spring框架](#第1章-spring框架)
  * [第2章 模型和MVC模式](#第2章-模型和mvc模式)
    * [2.1 模型1](#21-模型1)
    * [2.2 模型2](#22-模型2)
    * [模型2之Servlet控制器](#模型2之servlet控制器)
    * [模型2之Filter分发器](#模型2之filter分发器)
    * [校验器](#校验器)
    * [依赖注入](#依赖注入)
  * [第3章 Spring MVC](#第3章-spring-mvc)
    * [3.1采用Spring MVC的好处](#31采用spring-mvc的好处)
    * [Spring MVC 的DispatcherServlet（前置控制器）](#spring-mvc-的dispatcherservlet（前置控制器）)
    * [Controller 接口](#controller-接口)
    * [视图解释器](#视图解释器)
  * [第4章 基于注解的控制器](#第4章-基于注解的控制器)
    * [4.1 注解类型](#41-注解类型)
      * [4.1.1 Controller注解类型](#411-controller注解类型)
      * [4.1.2 RequestMapping注解类型](#412-requestmapping注解类型)
      * [4.1.3 Autowired注解类型](#413-autowired注解类型)
    * [4.2 编写请求处理方法](#42-编写请求处理方法)
    * [4.3应用@Autowired和@Service进行依赖注入](#43应用autowired和service进行依赖注入)
    * [4.4 重定向与Flash属性](#44-重定向与flash属性)
    * [4.5 请求参数和路径变量](#45-请求参数和路径变量)
    * [@ModelAttribute](#modelattribute)
    * [父子上下文(WebApplicationContext)](#父子上下文webapplicationcontext)
    * [springMVC-mvc.xml 配置文件片段讲解](#springmvc-mvcxml-配置文件片段讲解)
    * [如何访问到静态的文件，如jpg,js,css？](#如何访问到静态的文件，如jpgjscss？)
    * [请求如何映射到具体的Action中的方法？](#请求如何映射到具体的action中的方法？)
    * [Model、ModelMap及ModelAndView之间的区别](#model、modelmap及modelandview之间的区别)
  * [第5章 数据绑定和表单标签库](#第5章-数据绑定和表单标签库)
    * [5.1 数据绑定概览](#51-数据绑定概览)
    * [5.2 表单标签库](#52-表单标签库)
      * [5.2.1 表单标签](#521-表单标签)
      * [5.2.2 input标签](#522-input标签)
      * [5.2.3 password 标签](#523-password-标签)
      * [5.2.4 hidden标签](#524-hidden标签)
      * [5.2.5 textarea标签](#525-textarea标签)
      * [5.2.6 checkbox 标签](#526-checkbox-标签)
      * [5.2.7 radiobutton标签](#527-radiobutton标签)
      * [5.2.9 radiobuttons标签](#529-radiobuttons标签)
      * [5.2.10 select标签](#5210-select标签)
      * [5.2.11 option标签](#5211-option标签)
      * [5.2.12 options标签](#5212-options标签)
      * [5.2.13 errors标签](#5213-errors标签)
  * [第6章 转换器和格式化](#第6章-转换器和格式化)
    * [6.1 Converter转换器](#61-converter转换器)
    * [6.2 Formatter](#62-formatter)
    * [6.3 用Register注册Formatter](#63-用register注册formatter)
    * [6.4 选择Converter，还是Formatter](#64-选择converter，还是formatter)
  * [第7章 验证器](#第7章-验证器)
    * [7.1 验证概览](#71-验证概览)
    * [7.2 Spring验证器](#72-spring验证器)
    * [7.3 ValidationUtils 类](#73-validationutils-类)
    * [7.4 JSR 303验证](#74-jsr-303验证)
  * [第8章 表达式语言](#第8章-表达式语言)
    * [8.1 表达式语言的语法](#81-表达式语言的语法)
                        * [](#)
      * [8.1.1 关键字](#811-关键字)
      * [8.1.2 []和.运算符](#812-[]和运算符)
    * [8.1.3 取值规则](#813-取值规则)
  * [8.2 访问JavaBean](#82-访问javabean)
  * [8.3 EL隐式对象](#83-el隐式对象)
    * [8.3.1 pageContext](#831-pagecontext)

# SpringMVC 学习笔记
## 前言
1. 什么是MVC？\
    MVC是Model-View-Controller 的缩写。它是一个广泛应用于图形化用户交互开发中的设计模式。\
2. 互联网用户访问Web服务器请求资源的流程:   
    1. 互联网用户点击或者输入一个URL地址来访问一个资源
例如：http://baidu.com/index.html
Url的第一个部分是http协议，除http协议外还可以采用其他类型的协议，如：
mailto:joe@example.org
ftp://marketing@ftp.example.org\
通常，http的URL格式如下：
protocol：//[host.]domain[:port][/context][/resource][?query String | path variable]\
或者：
protocol://IP Address[:port][/context][/resource][?query String | path variable]


**由于IP地址不容易被记忆，所以实践中更倾向于使用域名。一台计算机可以托管不止一个域名，因此，不同域名可能指向同一个IP。另外，example.com或者example.org无法被注册，因为它们被保留用于各文档手册举例使用。**\

**注：**
- www 是默认的主机名。通常, http://www.domainName 会被映射到http://domainName。
- Http的默认端口是80端口。因此，对于采用80端口的Web服务器，无需输入端口号。若web服务器不运行在80端口，必须输入相应端口号，例如Tomcat服务器默认端口号是8080。
- localhost 作为一个保留关键字，用于指向本机。
- context部分用来代表应用名称，该部分也是可选的。一台Web服务器可以允许多个上下文（应用），其中一个可以配置为默认上下文。若访问默认上下文中的资源可以跳过Context部分。
- 一个context可以有一个或多个默认资源（通常为index.html、index.htm或者default.htm)。一个不带资源名称的URL通常指向默认资源，当存在多个资源时，其中最高优先级的资源将被返回给客户端。
- 资源名侯可以有一个或者多个查询够或者路径参数。查询语句是一个Key/Value组，多个查询语句间用“&”符号分隔。路径参数类似于查询语句，但只有value部分，多个value部分用“/”符号分隔。

3.http请求响应?\
一个HTTP请求包含三个部分内容：
1. 方法-URI-协议/版本。
2. 请求头信息。
3. 请求正文。

**其中：**

**请求方法** \
在 HTTP 1.1 规范中定义了7种类型的方法，包括GET、POST、HEAD、OPTIONS、PUT、DELETE以及TRACE，其中GET和POST广泛应用于互联网。

**URI** 
定义了一个互联网资源，通常解析为服务器根目录的相对路径。因此，通常用“/”符号打头。另外，URL是URI的一个具体类型。

**HTTP**
请求包含的请求头信息，包括关于客户端环境以及实体内容等非常有用的细腻些。每个header都用回车/换行（即CRLF)分隔。HTTP服务器根据此判断请求正文的起始位置，一些书籍将CRLF称为HTTP请求的第四种组件。


4.HTTP响应?\
HTTP响应包含三个部分。
1. 协议-状态码-描述
2. 响应头信息。
3. 响应正文。


第一行是协议及版本号HTTP1.1

**状态码**\
当浏览者访问一个网页时，浏览者的浏览器会向网页所在服务器发出请求。当浏览器接收并显示网页前，此网页所在的服务器会返回一个包含HTTP状态码的信息头（server header）用以响应浏览器的请求。\
常见的HTTP状态码：
- 200 - 请求成功
- 301 - 资源（网页等）被永久转移到其它URL
- 404 - 请求的资源（网页等）不存在
- 500 - 内部服务器错误

HTTP状态码分类 \
HTTP状态码由三个十进制数字组成，第一个十进制数字定义了状态码的类型，后两个数字没有分类的作用。HTTP状态码共分为5种类型：

分类 | 分类描述
---|---
1** | 信息，服务器收到请求，需要请求者继续执行操作
2** | 成功，操作被成功接收并处理
3** | 重定向，需要进一步操作以完成请求
4** | 客户端错误，请求包含语法错误或者无法完成请求
5** | 服务器错误，服务器在处理请求的过程中发生了错误

> http://tool.oschina.net/commons?type=5\
详细状态码查看该链接


5为什么SUN发布Servlet和JSP代替CGI技术？
CGI存在的问题：\
CGI技术为每个请求创建相应的进程，但是创建进程会耗费大量的CPU周期，导致很难编写==可伸缩==的CGI程序。\
相较于CGI，Servlet则快了很多，因为当一个Servlet为响应第一次请求而创建后，会驻留在内存中，以便响应后续请求。\
**JSP**(JavaServer Pages)技术的产生是用来简化Servlet开发。\
Servlet是允许在Servlet容器中的Java程序，而Servlet容器或者Servlet引起相当于一个Web服务器，但可以动态产生内容而不仅是静态资源。\
**关系**\
一个Servlet是一个Java程序，一个Servlet应用包含了一个或多个Servlet，一个JSP页面会被翻译并编译成一个Servlet。
一个Servlet应用允许在一个Servlet容器中，它无法独立允许。Servlet容器将来自用户的请求传递给Servlet应用，并将Servlet应用的响应返回给用户。由于大部分Servlet应用都会包含一些JSP界面，故称Java Web应用为“Servlet/JSP应用架构。”

Web服务器和Web客户端基于HTTP通信。因此，Web服务器往往成为HTTP服务器。\
允许一个Java企业版应用需要一个Java企业不能容器，常见的容器有Glass Fish、JBoss、Oracle Weblogic。\
**注**\
Tomcat和Jetty不是Java企业版容器，故他们不能允许EJB或JMS。

6、Maven和Gradle是什么？\
现在的应用通常有很多的依赖包，并且这些依赖包也有自己的依赖包。通过一个依赖管理工具，可以把我们从解析和下载依赖的工作中解放出来，并且这些工具还可以帮助我们构建、测试和打包应用。Maven和Gradle是当下流行的一俩管理系统。

----
## 第1章 Spring框架
7、依赖注入（DI）是什么？
场景1：有两个组件A和B，A依赖于B。假设A是一个类，且A有一个方法importantMethod用到了B，如下：

```
public class A{
    public void importantMethod{
        B b = new B(); // get an instance of B
        b.usefulMethod();
        ...
    }
    ...
}
```
B可能是一个具体类，也可能是接口，若B有多个实现，问题将变得复杂，若选择任意一个接口B的实现类，则降低了A的重用性。\
依赖注入：接管对象的创建工作，并将该对象的引用注入需要该对象的组件。
DI在本例会分别创建对象A、B，将对象B注入到对象A中。\
为了能让框架进行依赖注入，要在A中编写特定的B的setter方法或者构建方法。该方法会被框架调用，以注入B的实例，由于对象依赖由依赖注入，类A的importantMethod 方法不再需要在调用B的usefulMethod方法之前创建一个B的实例。\

```
public class A{
    private B b;
    public void importantMethod(){
        // no need to worry about creating B anymore
        // B b = new B();// get an instance of B
        b.usefulMethod();
        ...
    }
    public void setterB(B b){
        this.b = b;
    }
}
```
也可以用构造器方式注入：
```
public class A{
    private B b;
    
    public A(B b){
        this.b = b
    }
    
    public void importantMethod(){
        // no need to worry about creating B anymore
        // B b = new B();// get an instance of B
        b.usefulMethod();
        ...
    }
}
```



**注**
- Spring管理的对象成为Beans。
- 使用Spring，即是将几乎所有重要对象的创建工作交给Spring，并配置如何注入依赖。
- Spring支持XML或注解两种配置方式。
- Spring需要创建一个ApplicationContext对象，代表一个Spring控制反转容器，org.springframework.context.ApplicationContext接口有多个实现，如ClassPathXmlApplicationContext和FileSystemXmlApplicationContext。这两个实现都需要传入一个包含beans信息的XML文件。
- ClassPathXmlApplicationContext尝试在类加载路径中加载配置文件；而FileSystemXmlApplicationContext尝试在文件系统中加载。
- 可以通过调用ApplicationContext的getBean方法获得对象。getBean方法会查询id对应且类型对应的bean对象。
- 理想情况下，我们只需要在测试代码中创建一个ApplicationContext，应用程序本身无需处理；对于SpringMVC应用，可以通过一个Spring Servlet来处理ApplicationContext，而无需直接处理。

8. 如何配置Spring的ApplicationContext.XML文件？
配置文件通常根元素为beans：
```
<?xml version = '1.0' encoding = "UTF"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        ...
</beans>
```

**注**
- 如果需要更强的Spring配置能力，可以在schema location 属性中添加响应的schema，也可以指定schema版本，一般推荐默认schema，以便升级Spring库时无需更改配置文件。
- 配置文件可以是一份也可以是多分，以支持模块化配置。ApplicationContext的实现类支持读取多分配置文件。另一种方式是通过一份主配置文件，将该文件导入到其他配置文件。\
示例：
```
<?xml version = '1.0' encoding = "UTF"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <import resource = "config1.xml"/>
        <import resource = "module2/config2.xml"/>
        <import resource = "/resources/config3.xml"/>
        
        ...
</beans>
```

9. Spring如何管理bean和依赖关系?
    1. 通过构造器创建一个bean实例
    通过调用ApplicationContext的getBean方法可以获取一个bean的实例。示例：
```
<?xml version = '1.0' encoding = "UTF"?>
<beans xmlns = "http://www.springframework.org/schema/beans"
    xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation = "http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <bean name="product" class = "springintro.bean.Product"/>
</beans>
```
该bean的定义告诉Spring框架，**通过默认的无参构造器来初始化Product类**。如果不存在该构造器（重载了该构造器或者没有显示声明默认构造器[声明了构造器后，系统不自动声明默认构造器],则Spring将抛出一个异常。此外无参构造器并不要求是public的。

**注意**
- 应采用id或者那么属性标识一个bean。为了让Spring创建一个Product实例，应将bean定义的那么值“product”（也可以是id值）和Product类型作为参数传递给ApplicationContext的getBean方法。

```
ApplicationContext context = new ClassPathXmlApplicationContext(new String[]{"spring-config.xml"});
product product1 = context.getBean("product",Product.class);
product1.setName("Excellent snake oil");
System.out.println("product1:" + product1.getName());

```
   ii. 通过工厂方法创建一个bean实例
   
   - Spring支持通过一个工厂方法来初始化类。
   实例：
```
<bean id = "localDate" class="java.time.LocalDate" factory-method = "now"/>
```

```
ApplicationContext context = new ClassPathXmlApplicationContext(
    new String[] {"spring-config.xml"});
    LocalDate locaDate = context.getBaen("localDate",LocalDate.class);
```

iii. 销毁方法的使用\
当我们希望类在被销毁前能执行一些方法，可以在bean定义中配置destroy-method属性，来指定在销毁前要执行的方法。

示例：在配置Spring时，通过java.util.concurrent.Executors的静态方法newCachedThreadPool来创建一个Java.util.concurrent.ExecutorService实例，并指定了destroy-method属性值为shutdown方法。
```
<bean id = "executorService" class = "java.util.concurrent.Executors"
    factory-method = "newCachedThreadPool"
    destory-method = "shutdown" />
```

iv. 向构造器传递参数
Spring支持通过带参数的构造器来初始化类。
示例：
```
package springintro.bean;
import java.io.Serializable;

public class Product implements Serializable {
    private static final long serialVersionUID = 748392348L;
    private String name;
    private String description;
    private float price;
    
    public Product(){
        
    }
    
    pubilc Product(String name, String description, float price){
        this.name = name;
        this.description = description;
        this.price = price;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setDesciption(String description) {
        this. description = description;
    }
    public float getPrice(){
        return price;
    }
    public void setPrice(float price){
        this.price = price;
    }
}

---------------------------------------------
<bean name = "featuredProduct" class = "springintro.bean.Product">
    <constructor-arg index = "0" value = "Ultimate Olive Oil"/>
    <constructor-arg index = "1"
        value = "The purest oilve oil on the market"/>
    <constructor-arg index = "2"
        value = "9.95"/>
</bean>

```
**注意**
采用这种方式，对应构造器必须传递所有参数。

v. Setter方式依赖注入
示例：
```
package springintro.bean;

public class Employee{
    private String firstname;
    private String lastname;
    private Address homeAddress;
    
    public Employee(){
        
    }
    
    public Employee(String firstName, String lastName, Address homeAddress){
        this.firstName = firstName;
        this.lastName = lastName;
        this.homeAddress = homeAddress;
    }
    
    public String getFirstName(){
        return firstName;
    }
    
    public void setFirstName(String firstName){
        this.firstName = firstName;
    }
    
    public String getLastName(){
        return lastName;
    }
    
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }
    
    public Address getHomeAddress(){
        return home Address;
    }
    
    public void setHomeAddress(Address homeAddress){
        this.homeAddress = homeAddress;
    }
    
    @Override
    public String toString(){
        return firstName + " " + lastName + "\n" + homeAddress;
    }
}

----------------------------------------------
Address.java

package springintro.bean;

public class Address {
    private String line1;
    private String line2;
    private String city;
    private String state;
    private String ZipCode;
    private String country;
    
    public Address(String line1, String line2, String city, String state, String ZipCode, String country){
        this.line1 = line1;
        this.line2 = line2;
        this.city = city;
        this.state = state;
        this.zipCode = zipCode;
        this.country = country;
    }
    
    // getters and setters omitted
    
    @Override
    public String toString(){
        return line1 + "\n"
            + line2 + "\n"
            + city + "\n"
            + state + " " + zipCode+ "\n"
            + country;
    }
}

------------------------------------------------------
ApplicationContext.xml
...Spring 头
<bean name = "simpleAddress" class = "springintro.bean.Address">
    <constructor-arg name = "line1" value = "151 Corner Street"/>
    <constructor-arg name = "line2" value = ""/>
    <constructor-arg name = "city" value = "Albany"/>
    <constructor-arg name = "state" value = "NY"/>
    <constructor-arg name = "zipCode" value = "99999"/>
    <constructor-arg name = "country" value = "US"/>
</bean>

<bean name = "employee1" class = "springintro.bean.Employee">
    <property name = "homeAddress" ref = "simpleAddress"/>
    <property name = "firstName" value = "Junior"/>
    <property name = "lastName" value = "Moore"/>
</bean>

```

- simpleAddress 对象是Address类的一个实例，它通过构造器方式实例化。
- employee1 对象通过property来调用setter方法以设置值，须注意的是，homeAddress属性配置的是simpleAddress对象的引用。
- 被引用兑现过的配置定义无需早于引用其对象的定义。
    
vi. 构造器方式依赖注入
上述代码中Employee 类提供了一个可以传递参数的构造器，可以将Address对象通过构造器注入：
```
<bean name = "employee2" class = "springintro.bean.Employee">
    <constructor-arg name = "firstName" value = "Senior"/>
    <constructor-arg name = "lastName" value = "Moore"/>
    <constructor-arg name = "homeAddress" ref = "simpleAddress"/>
</bean>

<bean name = "simpleAddress" class= "springintro.bean.Employee">
    <constructor-arg name = "line1" value = "151 Corner Street"
    <constructor-arg name = "line2" value = ""/>
    <constructor-arg name = "city" value = "Albany"/>
    <constructor-arg name = "state" value = "NY"/>
    <constructor-arg name = "zipCode" value = "99999"/>
    <constructor-arg name = "country" value = "US"/>
</bean>
```


----
## 第2章 模型和MVC模式
Java Web应用开发中有两种设计模式，分别成为模型1与模型2。
- 模型1是以页面为中心，适合于小应用开发。
- 模型2是基于MVC模式，是Java Web应用的推荐框架。

### 2.1 模型1
直接通过链接的方式进行JSP页面间的跳转，这种方式在中大型应用中会带来维护上的问题，修改一个JSP页面的名字会导致页面中大量的链接需要修正。

### 2.2 模型2
模型2是基于MVC模式（模型-视图-控制器）
- 视图负责应用的展示
- 模型风钻过了应用的数据及业务逻辑。
- 控制器负责接收用户输入，改变模型及视图的显示。

**注**
- Servlet或者Filter都可以充当控制器。
- Struts 1、Spring MVC、JavaServer Face是使用一个Servlet作为控制器，而Struts 2 则使用Filter作为控制器，
- 视图大部分采用JSP页面。
- 模型采用POJO（Plain Old Java Object），不同于EJB等特定的对象，POJO是一个普通对象。实践会采用一个JavaBean来持有模型状态，并将业务逻辑放到一个Action类里。

![模型2结构图](https://s2.ax1x.com/2019/02/20/k28oz4.png)

每个HTTP请求都发给Controller，请求中的URI标识出对应的action。action代表了应用可以执行的一个操作，一个提供了action的Java对象成为action对象。一个action类可以支持多个action（Spring MVC及Struts 2 ）,或者一个action（Struts）。

当执行多个action时，可以通过URI方式告诉控制器执行响应的action，然后将模型对象放到视图可以访问的区域，最后控制器利用RequestDispatcher或者HttpServletResponse.sendRedirect()方法跳转到视图（JSP页面或者其他资源）。在JSP页面中，用表达式语言以及定制标签显示数据。

**注意**
调用RequestDispatcher.forward方法或者HttpServletResponse.sendRedirect()方法并不会停止执行生于的代码，因此若forward方法不是最后一行diamagnetic，则应显示地返回。

```
if(action.equals(...)){
    RequestDispatcher rd = request.getRequestDispatcher(dispatchUrl);
    rd.forward(request, response);
    return; //explicitly return. Or else, the code below will be executed
}
// do something else

```

大多数情况，使用RequestDispatcher转发到视图，因为它比sendRedirect 更快响应。（这是因为重定向将导致服务器向浏览器发送状态代码为302的Http响应，并包含新的URL。而浏览器在接收到状态码302时，根据响应头部找到的URL向服务器发出新的HTTP请求。简言之，重定向需要一个往返，这使其转发较慢。）

那么使用重定向超过转发的优势时什么？
- 通过重定向，你可以将浏览器定向到其他应用程序。
- 由于在转发之后，浏览器中URL仍然指向开始页面，此时如果重载当前页面，开始页面将会被重新调用。比如用户在付款完成后不小心按下了刷新或者重新加载按钮，则又会重新要求用户付款，这显然时不合理的，因此采用重定向的方式

### 模型2之Servlet控制器

处理请求和发送响应的过程是由一种叫做Servlet的程序来完成的，并且Servlet是为了解决实现动态页面而衍生的东西。
- Tomcat 是Web应用服务器,是一个Servlet/JSP容器. Tomcat 作为Servlet容器,负责处理客户请求,把请求传送给Servlet,并将Servlet的响应传送回给客户
- Servlet是一种运行在支持Java语言的服务器上的组件. Servlet最常见的用途是扩展Java Web服务器功能,提供非常安全的,可移植的,易于使用的CGI替代品。
- 浏览器发送过来的请求也就是request，我们响应回去的就用response，均为请求文本。

**注**
Tomcat服务器响应客户请求的过程：
![Tomcat服务器响应客户请求](https://s2.ax1x.com/2019/02/20/k25vrQ.png)
1. Tomcat将http请求文本接收并解析，然后封装成HttpServletRequest类型的request对象，所有的HTTP头数据都可以通过request对象调用对应的方法查询到。
2. Tomcat同时会将响应的信息封装为HttpServletResponse类型的response对象，通过设置response属性就可以控制要输出到浏览器的内容，然后将response交给tomcat，tomcat就会将其变成响应文本的格式发送给浏览器。
3. Java Servlet API 是Servlet容器(tomcat)和servlet之间的接口，它定义了serlvet的各种方法，还定义了Servlet容器传送给Servlet的对象类，其中最重要的就是ServletRequest和ServletResponse。所以说我们在编写servlet时，需要实现Servlet接口，按照其规范进行操作。
4. 客户端通常只有GET和POST两种请求方式，Servlet为了响应这两种请求，必须重写doGet()和doPost()方法。大部分时候，Servlet对于所有的请求响应都是完全一样的，此时只需要重写service()方法即可响应客户端的所有请求。另外HttpServlet有两个方法:
- init(ServletConfig config):创建Servlet实例时，调用该方法初始化Servlet资源。

- destory():销毁Servlet实例时，自动调用该方法回收资源。

```
package appdesign1.controller;

import appdesign1.form.ProductForm;
import appdesign1.model.Product;
import appdesign1.action.SaveProductAction;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

import java.math.BigDecimal;
import java.rmi.server.ServerCloneException;

@WebServlet(name = "ControllerServlet", urlPatterns = {"/input-product", "/save-product"})
public class ControllerServlet extends HttpServlet {
    private static final long serialVersionUID = 1579L;

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
        process(request, response);
    }

    @Override
    public void doPost (HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException{
        process(request,response);
    }

    private void process(HttpServletRequest request, HttpServletResponse response)throws  IOException,ServletException{
        String uri = request.getRequestURI();
        /*
         * uri is in this form: /contextName/resourceName
         * for example: /appdesign1/input-product.
         * However, in the event of a default context, the context name is empty, and uri has this form
         * /resourceName, e.g.:/input-product
         */

        int lastIndex = uri.lastIndexOf("/");
        String action = uri.substring(lastIndex + 1);
        // execute an action
        String dispatchUrl = null;
        if("input-product".equals(action)){
            // no action class , just forward
            dispatchUrl = "/jsp/ProductForm.jsp";
        }else if("save-product".equals(action)){
            //create form 创建表单对象
            ProductForm productForm = new ProductForm();
            //populate action properties
            productForm.setName(request.getParameter("name"));
            productForm.setDescription(request.getParameter("description"));
            productForm.setPrice(request.getParameter("price"));

            // create model 创建模型对象，通过表单对象设置相应属性
            Product product = new Product();
            product.setName(productForm.getName());
            product.setDescription(productForm.getDescription());
            try{
                product.setPrice(new BigDecimal(productForm.getPrice()));
            } catch (NumberFormatException e) {
                System.out.println("Number Format ERROR!");
            }

            //execute action method 执行针对module对象的业务逻辑
            SaveProductAction saveProductAction = new SaveProductAction();
            saveProductAction.save(product);

            // store model in a scope variable for the view
            // 转发请求到视图（JSP页面）
            request.setAttribute("product",product);
            dispatchUrl = "/jsp/ProductDetails.jsp";

        }
        //forword to a view
        if(dispatchUrl != null){
            RequestDispatcher rd = request.getRequestDispatcher(dispatchUrl);
            rd.forward(request, response);
        }
    }
}

```

### 模型2之Filter分发器
servlet是最常见的控制器，但过滤器也可以作为控制器。需要注意的是，过滤器没有作为欢迎界面的权限。即输入域名之后不会调用过滤器分派器。
过滤器过滤目标是包含静态网址在内的所有网址，因此若没有相应的action则需要调用filterChain.doFilter()。

### 校验器
在web应用执行相应action时，很重要的一个步骤就是进行输入校验，校验的内容可难可易，在现代MVC框架通常支持两种校验方法：
- 编程式：需要通过编码进行用户输入校验
- 声明式：需要提供包含校验规则的XML文档或者属性文档。

**注意**
即使可以用HTML5和JavaScript执行客户端验证，但用户可以很轻易绕过校验，所以最好还是执行服务器输入验证。

### 依赖注入
依赖注入有两种方式：
- 为每个依赖关系创建一个set方法，通过set方法传递依赖类（接口）的具体实现。
- 通过构造方法或者类属性进行注入。

## 第3章 Spring MVC
### 3.1采用Spring MVC的好处
若基于某个框架来开发一个模型2的应用程序，我们要负责编写一个Dispatcher servlet 和控制类。\
Dispatcher servlet必须要做到如下事情：
- 根据URI调用相应的action
- 实例化正确的控制器
- 根据请求参数来构造表单bean。
- 调用控制器对象的相应方法。
- 转向到一个视图

Spring MVC是一个包含了Dispatcher servlet 的MVC框架。它调用控制器方法并转发到视图。
使用Spring MVC开发的好处：
- Spring MVC 提供了一个Dispatcher Servlet，无需额外开发。
- Spring MVC使用基于XML的配置文件，可以编辑，而无需重新编译应用程序。
- Spring MVC 实例化控制器，并根据用户的输入来构造bean
- Spring MVC 可以自动绑定用户输入，并正确地转换数据类型，例如Spring MVC能自动解析字符串，并设置float或decimal类型的属性。
- Spring MVC 可以校验用户输入，若校验不通过，则重定向回输入表单。输入建议是可选的，支持编程方式以及生命方式。Spring MVC 内置了常见的校验器
- Spring MVC 是 Spring框架的一部分，可以利用Spring提供的其他能力。
- Spring MVC 支持国际化和本地化，支持根据用户区域显示多国语言。
- Spring MVC支持多种视图技术。最常见的JSP技术以及其他技术包括Velocity和FreeMarker。

### Spring MVC 的DispatcherServlet（前置控制器）
Spring MVC自带一个开箱即用的Dispatcher Servlet，该Servlet的全名是org.springframework.web.servlet.DispatcherServlet。

要使用这个servlet，需要在部署描述符（web.xml文件）中使用servlet和servlet-mapping元素来配置它，如下：
```
<servlet>
    <servlet-name>springmvc</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springmvc</servlet-name>
    <!-- map all requests to the DispatcherServlet -->
    <url-parttern>/</url-pattern>
</servlet-mapping>


```
其中的on-startup元素是可选的。如果它存在，则它将在应用程序启动时装在servlet并调用它的init方法。若它不存在，则该servlet的第一个请求时加载它。\
<load-on-startup>1</load-on-startup>是启动顺序，让这个Servlet随Servletp容器一起启动。\

Servlet可以有多个，是通过名字来区分的。每一个DispatcherServlet有自己的WebApplicationContext上下文对象。

Dispatcher servlet将使用Spring MVC诸多默认的组件。此外初始化时，它会寻找子啊应用程序的WEB-INF目录下的一个配置文件，命名规则如下：
servletName-servlet.xml
此处的servletName时上诉代码中servlet-name对应的名字

也可以把Spring MVC的配置文件放在应用程序的各个地方，只要告诉Dispatcher servlet在哪里找到该文件。**使用servlet标签中添加<init-param>元素，该元素拥有一个值为contextConfigLocation的param-name元素，其param-value元素则包含配置文件的路径。没有的话，就默认到/WEB-INF文件夹下，并按照通常的命名约定。**

指明了配置文件的文件名，不使用默认配置文件名，而使用springMVC.xml配置文件。

其中<param-value>**.xml</param-value> 这里可以使用多种写法
1. 不写,使用默认值:/WEB-INF/<servlet-name>-servlet.xml
2. <param-value>/WEB-INF/classes/springMVC.xml</param-value>
3. <param-value>classpath*:springMVC-mvc.xml</param-value>
4. 多个值用逗号分隔

这段告诉了Servlet/JSP容器，我们将使用Spring MVC的Dispatcher Servlet，并通过 url-pattern元素值配置为“/”,将所有的URL映射到该servlet。

<url-pattern>*.form</url-pattern> 会拦截 *.form结尾的请求。

Servlet拦截匹配规则可以自已定义，拦截哪种URL合适？ 
当映射为@RequestMapping("/user/add")时，为例：

1、拦截 *.do、 *.htm， 例如：/user/add.do

这是最传统的方式，最简单也最实用。不会导致静态文件（jpg,js,css）被拦截。

 

2、拦截/，例如：/user/add

可以实现现在很流行的REST风格。很多互联网类型的应用很喜欢这种风格的URL。

弊端：会导致静态文件（jpg,js,css）被拦截后不能正常显示。想实现REST风格，事情就是麻烦一些。后面有解决办法还算简单。

3、拦截/*，这是一个错误的方式，请求可以走到Action中，但转到jsp时再次被拦截，不能访问到jsp。

### Controller 接口
在Spring 2.5之前，开发一个控制器的唯一方法时实现 org.springfranmework.web.servlet.mvc.Controller接口。这个接口公开了一个handleRequest方法。
```
ModelAndView handleRequest(HttpServletRequest 
        request, HttpServletResponse response)

```
其实现类可以访问对应请求的HttpServletRequest和HttpServletResponse,还必须返回一个ModelAndView对象，它包含视图路径或视图路径和模。

Controller接口的实现类只能处理一个单一动作(action)，而一个基于注解的控制器可以同时支持多个请求处理动作，并且无需实现任何接口。

### 视图解释器
Spring MVC中的视图解析器负责解析视图。可以通过在配置文件中定义一个ViewResolver来配置视图解析器。
```
<bean id = "viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name = "prefix" value="/WEB_INF/jsp/"/>
    <property name = "suffix" value=".jsp"/>
</bean>

```
视图解析器配置有前缀和后缀两个属性。这样依赖View的路径缩短，不必再将视图路径完全写出，只需提供视图名，视图解析器会自动增加前后缀。

**注意** 要使用非默认配置文件的命名和路径，需要使用明媚context Config Location的init-param,其值应为配置文件在应用中的相对路径（相对于web.xml）。

## 第4章 基于注解的控制器
### 4.1 注解类型
使用基于注解的控制器有几个有点：
- 控制器类可以处理多个动作，若实现Controller接口的一个操作器只能处理一个动作。这就允许将相关的操写在同一个控制器类中，从而减少应用程序中类的数量。
- 基于注解的控制器的请求映射不需要存储在配置文件中。使用RequestMapping注释类型可以对一个方法进行请求处理。
- controller和RequestMapping注释类型是Spring MVC API最重要的两个注解类型。

#### 4.1.1 Controller注解类型
添加了@Controller注解注解的类就可以担任控制器（Action）的职责\
org.springframework.stereotype.Controller注解类型用于指示Spring类的实例是一个控制器。
Spring使用扫描机制来找到应用程序中所有基于注解的控制器类，为了保证Spring能找到控制器需要完成两件事：
1. 在Spring MVC 的配置文件中声明spring-context，如下所示：<beans ... xmlns:context = "http://www.springframework.org/schema/context" ...>
2. 然后需要应用<component-scan/>元素，如下所示：
<context:component-scan base-package = "basePackage"/>

请在<component-scan/>元素中指定控制器类基本包下，并且不要指定一个太宽泛的基本包

#### 4.1.2 RequestMapping注解类型
我们需要在控制类的内部为每个动作开发相应的处理方法，要让Spring指定用什么方法处理动作，需要使用 org.springframework.web.bind.annotation.RequestMapping注解类型映射的URI与方法。
RequestMapping注解类型的作用同其名字所暗示的：映射一个请求和一种方法。可以使用@RequestMapping注解一种方法或类。

一个采用@RequestMapping注解的方法将成为一个处理请求的方法，并由调度程序在接收到对应的URL时调用。

```
package com.example.controller;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
...

@Controller
public class CustomerController{
    
    @RequestMapping(value = "/input-customer")
    public String inputCustomer(){
        //do something here
        return "CustomerForm";
    }
}
```
使用RequestMapping注解的value属性将URI映射到方法。上述例子就是将input-customer映射到inputCustomer方法。这样可以直接使用URL访问inputCustomer方法。
> http://domain/context/input-customer

由于value属性时RequestMapping注释的默认属性，因此，若只有唯一的属性，则可以省略属性名称。如果有多个属性必须写入value属性名称。
@RequestMapping(value = "input-customer ")
@RequestMapping("/input-customer ")

请求的应设置可以时个空字符串，此时该方法会被映射到以下网页：
http://domian/context

RequestMapping除了具有value属性外，还有其他属性。例如method属性用来指示该方法仅处理哪些HTTP方法。

此外RequestMapping注解类型也可以用来注解一个控制器类，如下：
```
import org.springframework.stereotype.Controller;
...

@Controller
@RequestMapping(value="/customer")
public class CustomerController{
    
}

```
所有的方法都将映射为相对于类级别的请求。
例如前面Controller之后写RequestMapping("/customer"),在类里再写一个RequestMapping,value属性为/delete，那么URL就会映射到http://domain/context/customer/delete

#### 4.1.3 Autowired注解类型
@Autowired为Spring提供的注解，需要导入包org.springframework.beans.factory.annotation.Autowired;\
@Autowired写在字段和setter方法上，如果写在字段上，那么就不需要再写setter方法。
@ModelAttribute ：被该注解修饰的方法，会在每一次请求时优先执行，用于接收前台jsp页面传入的参数 
```
public class TestServiceImpl {
    // 下面两种@Autowired只要使用一种即可
    @Autowired
    private UserDao userDao; // 用于字段上
    
    @Autowired
    public void setUserDao(UserDao userDao) { // 用于属性的方法上
        this.userDao = userDao;
    }
}
```
@Autowired注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以结合@Qualifier注解一起使用。

```
public class TestServiceImpl {
    @Autowired
    @Qualifier("userDao")
    private UserDao userDao; 
}
```

### 4.2 编写请求处理方法
每个请求处理方法可以有多个不同类型的参数，以及一个多种类型的返回结果。例如，如果在请求处理方法种需要访问HttpSession对象，则可以添加的HttpSession作为参数，Spring会将对象正确地传递给方法。
```
@RequestMapping("/uri")
public String myMethod(HttpSession session){
    ...
    session.addAttribute(key,value);
    ...
}

```
如果要访问客户端语言环境和HttpServletRequest对象，则可以在方法签名上包括这样的参数：
@RequestMapping("/uri")
public String myOtherMethod(HttpServletRequest request, Locale locale){
    ...
    // access Locale and HttpServletRequest here
    ...
}

**在请求处理方法中可能出现的参数类型**
- javax.servlet.ServletRequest或javax.servlet.HttpServletRequest。
- javax.servlet.ServletRespone 或 javax.servlet.http.HttpServletResponse。
- javax.servlet.http.HttpSession。
- org.springframework.web.context.request.WebRequest 或 org.springframework.web.context.request.NativeWebRequest
- java.util.Locale
- java.io.InputStream 或 java.io.Reader
- java.io.OutputStream 或 java.io.Writer
- java.security.Principal。
- HttpEntity<?>paramters
- java.util.Map/org.springframework.ui.Model/。
- org.springframework.web.servlet.mvc.support.RedirectAttributtes
- org.springframework.validation.Errors/
- org.springframework.validation.BingdingResult。
- 命令或表单对象
- org.springframework.web.bind.support.SessionStatus。
- org.springframework.web.bind.support.SeesionStatus
- org.springframework.web.util.uriComponentsBuilder。
- 带@PathVariable、@MatrixVariable、@RequestParam、@RequestHeader、@ReqBody或@RequestPart注释的对象。

**注** 特别重要的时Model类型，这不是一个Servlet API类型，而是一个包含Map的Spring MVC类型。每次调用请求处理方法时，Spring MVC都创建Model对象并将其Map注入到各种对象中。

请求处理方法可以返回如下类型的对象：
- ModelAndView
- Model
- 包含模型的属性的Map
- View
- 代表逻辑视图名的String
- void
- 提供对Servlet的访问，以响应HTTp头部和内容HttpEntity或ResponseEntity对象
- Callable
- DeferredResult
- 其他类型，Spring将其视作输出给View的对象模型。

SpringMVC配置文件中，最主要的时<component-scant/>元素。这是要指示Spring MVC扫描目标包中的类，本例时controller包。
<annotation-driven/>元素做很多事情，其中包括注册用于控制器注解的bean对象。和两个<resources/>元素则指示哪些静态资源需要单独处理（不通过Dispatcher Servlet）
**第一个**确保在/CSS目录下的所有的文件可见。
**第二个**允许显示所有的.html文件。

**注意** 如果没有<annotation-driven/>,<resources/>元素会阻止任意控制器被调用。若不需要使用resources，则不需要<annotation-driven/>元素

```

```
其中，ProductController的saveProduct方法的第二个参数时org.springframework.ui.Model类型。无论是否会使用，Spring MVC都会在每一个请求处理方法被调用时创建一个Model实例，用于增加需要显示在视图中的属性。例如，通过调用model.addAttribute来添加Product实例：
model.addAttributr("product", product);

### 4.3应用@Autowired和@Service进行依赖注入
将依赖注入到SPring MVC控制器的最简单的方法时通过注解@Autowired到字段或者方法。Autowired注解类型术语org.springframework.beans.factory.annotation包。
此外，为了能作为依赖注入，类必须要注明为@Service。该类型时org.springframework.stereotype包的成员。Service注解类型指示类时一个服务。此外，在配置文件中，还需要添加一个<component-scan/>元素来扫描依赖基本包。\
<context:component-scan base-package = "dependencyPackage"/>


### 4.4 重定向与Flash属性
转发与重定向的区别
- 转发比重定向快，业务重定向经过客户端，转发没有。
- 若需要重定向到外部网站，无法使用转发。
- 重定向可以避免用户重新加载页面时再次调用同样的动作。
- 使用重定向无法轻松的传值给目标页面

可以通过redirect/forward:url方式转到另一个Action进行连续的处理。

可以通过redirect:url 防止表单重复提交 。\
写法如下：\
return "forward:/order/add";\
return "redirect:/index.jsp";\
住哪发可以简单地将属性添加到Model，是的目标视图可以轻松访问，由于重定向经过客户端，所以Model中地一切都将在重定向时丢失。

带参数重定向--RedirectAttributes

用户保存或修改后，为了防止用户刷新浏览器(F5)导致表单重复提交，一般在保存或修改操作之后会redirect到一个结果页面（不是forward），同时携带参数，如操作成功的提示信息。因为是Redirect，Request里的attribute不会传递过去。Spring在3.1才提供了这个能力--RedirectAttributes。 反复按F5，操作成功的提示信息也不会再次出来（总共只出现一次），效果很理想。

**解决方法** Spring3.1及以后地版本通过Flash属性提供以中重定向传值地方式。
> 要使用Flash属性，必须在Spring MVC配置文件中有一个<annotation-driven/>元素。然后，还必须在方法上添加一个新的参数类型org.springframework.web.servlet.mvc.support.RedirectAttributes。

```
public String save(@ModelAttribute("group") Group group, RedirectAttributes redirectAttributes) {  
    accountManager.saveGroup(group);  
    redirectAttributes.addFlashAttribute("message", "操作成功");  
    return "redirect:/account/group/";  
}  
```

### 4.5 请求参数和路径变量
请求参数和路径变量都可以用于发送值给服务器。二者都是URL地一部分。请求参数采用key=value形式，并以“&”分隔。
传统地servlet编程中，可以使用HttpServletRequest的getParameter方法来获取一个请求参数值。

String productID = httpServletRequest.getParameter("productID");

Sping MVC 提供org.springframework.web.bind.annotation.RequestParam注解类型来注解方法参数。例如，下面的方法包含了一个获取请求参数productID值的参数。

public void sendProduct(@Requestparam int productId)

**注** @RequestParam 注解的参数类型不一定时字符串。
路径变量类似请求参数，但没有key部分，只是一个值。
/view-product/productId
其中的productId时表示产品标识符的整数。在SpringMVC中，productId成为路径变量，用来发送一个值到服务器。

```
@RequestMapping(value = "/view-product/{id}")
public String viewProduct(@PathVariable Long id, Model model){
    Product product = productService.get(id);
    model.addAttribute("product",product);
    return "ProductView";
}
```

为了使用路径变量，需要在RequestMapping注解的值属性中添加一个变量，该变量必须放在花括号之间，然后在方法签名中添加一个同盟的变量，并加上@PathVariable注解。当该方法被调用时，请求URL的id值将被复制到路径变量中，并可以在方法中使用。路径变量的类型可以不是字符串。Spring MVC将尽力转换成一个非字符串类型。

可以在请求映射中使用多个路径变量。
@RequestMapping（value = "/view-product/{userId}/{orderId}"）

有时候，在使用路径变量时会遇到一个小问题：在某些情况下，浏览器可能会误解路径变量：
http://example.com/context/abc
浏览器会（正确）认为abc是一个动作。任何静态文件路径的解析。任何静态文件路径的解析，如CSS文件，将使用http://example.com/context作为基本路径。也就是说，若服务器发送的网页包含如下img元素：
<img src = "logo.png"/>
该浏览器将试图通过http://example.com/context/logo.png来加载logo.png
然而，若一个应用程序被部署为默认上下文（默认上下文路径是一个空字符串），则对于同一个目标的URL，会是这样的：
thhp://example.com/abc
下面是带有路径变量的URL：
http://example.com/abc/1
在这种情况下，浏览器会认为abc是上下文，没有动作。若在页面中使用<img src="logo.png"/>
浏览器将视图通过http://example.com/abc/log.png来寻找图像，并且它将找不到该图像。

可以通过使用JSTL标记的URL，标签会通过正确解析URL来修复该问题。
例如：
<style type = "text/css">@import url(css/main.css);</style>

修改为
    <style type = "text/css">
    @import url("<c:url value ="/css/main.css"/>");
    </style>

若程序部署为默认上下文，该链接标签会将URL转换为如下：
<style type = "text/css">@import url("/css/main.css");</style>

若程序不再默认上下文中，则它会被转换成为：
<style type = "text/css">@import url("/annotated2/css/main.css");</style>

### @ModelAttribute
在每次调用请求处理方法时，都会创建Model类型的一个实例，其实可以用ModelAttribute注解类型来访问Model实例，该注解类型也是org.springframework.web.bind.annotation包的成员。
可以用@ModelAttribute来注解方法参数或者方法。**@ModelAttribute 注解的方法会将其输入的或者创建的参数对象添加到Model对象中。**
例如，SpringMVC 将在每次调用submitOrder方法时创建一个Order实例。
```
@RequestMapping(method = RequestMethod.POST)
public String submitOrder(@ModelAttribute("newOrder") Order order, Model model){
    ...
}
```

**输入或者创建Order实例将会用newOrder键值添加到Model对象中。如果未定义键值名，则将使用该对象类型的名称。** 例如，每次调用如下方法时，会使用键值order 将Order实例添加到Model对象中。
public String submitOrder(@ModelAttribute Order order, Model model)

@ModelAttribute 的第二个用途是标注一个非请求的处理方法，被@ModelAttribute注解的方法会在每次调用该控制器类的请求处理方法时被调用。这意味着，如果一个控制器类有两个请求处理方法，以及一个有@ModelAttribute注解的方法，**该方法会优先调用，且的调用次数就会比每个处理请求方法更加频繁。**

Spring MVC会在调用请求处理方法之前调用带@ModelAttribute 注解的方法。带@ModelAttribute注解的方法可以返回一个对象或者一个void类型。**如果返回一个对象，则返回对象将自动添加到Model中。**
@ModelAttribute
public Product addProduct(@RequestParam String productId) {
    return productService.get(productId);
}

若方法返回void,则还必须添加一个Model类型的参数，并自行将实例添加到Model中，如下：
@ModelAttribute
public void populateModel(@RequestParam String id, Model model){
    model.addAttribute(new Account(id));
}

### 父子上下文(WebApplicationContext)
Spring会创建一个WebApplicationContext上下文，称为父上下文（父容器） ，保存在 ServletContext中，key是WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE的值。

可以使用Spring提供的工具类取出上下文对象：WebApplicationContextUtils.getWebApplicationContext(ServletContext);

DispatcherServlet是一个Servlet,可以同时配置多个，每个 DispatcherServlet有一个自己的上下文对象（WebApplicationContext），称为子上下文（子容器），子上下文可以访问父上下文中的内容，但父上下文不能访问子上下文中的内容。 它也保存在 ServletContext中，key是"org.springframework.web.servlet.FrameworkServlet.CONTEXT"+Servlet名称。当一个Request对象产生时，会把这个子上下文对象（WebApplicationContext）保存在Request对象中，key是DispatcherServlet.class.getName() + ".CONTEXT"。

可以使用工具类取出上下文对象：RequestContextUtils.getWebApplicationContext(request);

说明 ：Spring 并没有限制我们，必须使用父子上下文。我们可以自己决定如何使用。

**方案一，传统型：**

父上下文容器中保存数据源、服务层、DAO层、事务的Bean。
子上下文容器中保存Mvc相关的Action的Bean.
事务控制在服务层。
由于父上下文容器不能访问子上下文容器中内容，事务的Bean在父上下文容器中，无法访问子上下文容器中内容，就无法对子上下文容器中Action进行AOP（事务）。
当然，做为“传统型”方案，也没有必要这要做。

**方案二，激进型：**

Java世界的“面向接口编程”的思想是正确的，但在增删改查为主业务的系统里，Dao层接口，Dao层实现类，Service层接口，Service层实现类，Action父类，Action。再加上众多的O(vo\po\bo)和jsp页面。写一个小功能 7、8个类就写出来了。 开发者说我就是想接点私活儿，和PHP，ASP抢抢饭碗，但我又是Java程序员。最好的结果是大项目能做好，小项目能做快。所以“激进型”方案就出现了-----没有接口、没有Service层、还可以没有众多的O(vo\po\bo)。那没有Service层事务控制在哪一层？只好上升的Action层。

由于有了父子上下文，你将无法实现这一目标。解决方案是只使用子上下文容器，不要父上下文容器 。所以数据源、服务层、DAO层、事务的Bean、Action的Bean都放在子上下文容器中。就可以实现了，事务（注解事务）就正常工作了。这样才够激进。

总结：不使用listener监听器来加载spring的配置文件，只使用DispatcherServlet来加载spring的配置，不要父子上下文，只使用一个DispatcherServlet，事情就简单了，什么麻烦事儿也没有了。

 ### springMVC-mvc.xml 配置文件片段讲解 
 ```
 <?xml version="1.0" encoding="UTF-8"?>  
<beans  
    xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:tx="http://www.springframework.org/schema/tx"  
    xmlns:context="http://www.springframework.org/schema/context"    
    xmlns:mvc="http://www.springframework.org/schema/mvc"    
    xsi:schemaLocation="http://www.springframework.org/schema/beans   
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd   
    http://www.springframework.org/schema/tx   
    http://www.springframework.org/schema/tx/spring-tx-3.0.xsd  
    http://www.springframework.org/schema/context  
    http://www.springframework.org/schema/context/spring-context-3.0.xsd  
    http://www.springframework.org/schema/mvc  
    http://www.springframework.org/schema/mvc/spring-mvc-3.0.xsd">  
  
  
    <!-- 自动扫描的包名 -->  
    <context:component-scan base-package="com.app,com.core,JUnit4" ></context:component-scan>  
      
    <!-- 默认的注解映射的支持 -->  
    <mvc:annotation-driven />  
      
    <!-- 视图解释类 -->  
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">  
        <property name="prefix" value="/WEB-INF/jsp/"/>  
        <property name="suffix" value=".jsp"/><!--可为空,方便实现自已的依据扩展名来选择视图解释类的逻辑  -->  
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />  
    </bean>  
      
    <!-- 拦截器 -->  
    <mvc:interceptors>  
        <bean class="com.core.mvc.MyInteceptor" />  
    </mvc:interceptors>       
      
    <!-- 对静态资源文件的访问  方案一 （二选一） -->  
    <mvc:default-servlet-handler/>  
      
    <!-- 对静态资源文件的访问  方案二 （二选一）-->  
    <mvc:resources mapping="/images/**" location="/images/" cache-period="31556926"/>  
    <mvc:resources mapping="/js/**" location="/js/" cache-period="31556926"/>  
    <mvc:resources mapping="/css/**" location="/css/" cache-period="31556926"/>  
  
</beans>  
 ```
 > <context:component-scan/> 扫描指定的包中的类上的注解，常用的注解有：\
@Controller 声明Action组件\
@Service    声明Service组件\
@Service("myMovieLister")\
@Repository 声明Dao组件\
@Component   泛指组件, 当不好归类时. \
@RequestMapping("/menu")  请求映射\
@Resource  用于注入，( j2ee提供的 )\
默认按名称装配，@Resource(name="beanName") \
@Autowired 用于注入，(srping提供的) 默认按类型装配\
@Transactional( rollbackFor={Exception.class}) 事务管理
@ResponseBody\
@Scope("prototype")   设定bean的作用域

<mvc:annotation-driven /> 是一种简写形式，完全可以手动配置替代这种简写形式，简写形式可以让初学都快速应用默认配置方案。<mvc:annotation-driven /> 会自动注册DefaultAnnotationHandlerMapping与AnnotationMethodHandlerAdapter 两个bean,是spring MVC为@Controllers分发请求所必须的。
并提供了：数据绑定支持，@NumberFormatannotation支持，@DateTimeFormat支持，@Valid支持，读写XML的支持（JAXB），读写JSON的支持（Jackson）。

<mvc:interceptors/> 是一种简写形式。通过看前面的大图，知道，我们可以配置多个HandlerMapping。<mvc:interceptors/>会为每一个HandlerMapping，注入一个拦截器。其实我们也可以手动配置为每个HandlerMapping注入一个拦截器。

<mvc:default-servlet-handler/> 使用默认的Servlet来响应静态文件。

<mvc:resources mapping="/images/*" location="/images/" cache-period="31556926"/> 匹配URL/images/** 的URL被当做静态资源，由Spring读出到内存中再响应http。

### 如何访问到静态的文件，如jpg,js,css？
如果你的DispatcherServlet拦截"/"，为了实现REST风格，拦截了所有的请求，那么同时对*.js,*.jpg等静态文件的访问也就被拦截了。我们要解决这个问题。\
目的：可以正常访问静态文件，不可以找不到静态文件报404。

**方案一：激活Tomcat的defaultServlet来处理静态文件**
```
<servlet-mapping>   
    <servlet-name>default</servlet-name>  
    <url-pattern>*.jpg</url-pattern>     
</servlet-mapping>    
<servlet-mapping>       
    <servlet-name>default</servlet-name>    
    <url-pattern>*.js</url-pattern>    
</servlet-mapping>    
<servlet-mapping>        
    <servlet-name>default</servlet-name>       
    <url-pattern>*.css</url-pattern>      
</servlet-mapping>    
要配置多个，每种文件配置一个   
```
要写在DispatcherServlet的前面， 让 defaultServlet先拦截请求，这样请求就不会进入Spring了，我想性能是最好的吧。

Tomcat, Jetty, JBoss, and GlassFish 自带的默认Servlet的名字 -- "default"\
Google App Engine 自带的 默认Servlet的名字 -- "_ah_default"\
Resin 自带的 默认Servlet的名字 -- "resin-file"\
WebLogic 自带的 默认Servlet的名字  -- "FileServlet"\
WebSphere  自带的 默认Servlet的名字 -- "SimpleFileServlet" 

**方案二： **
在spring3.0.4以后版本提供了mvc:resources ，  使用方法：
```
<!-- 对静态资源文件的访问 -->    
<mvc:resources mapping="/images/**" location="/images/" />  
```
/images/**映射到ResourceHttpRequestHandler进行处理，location指定静态资源的位置.可以是web application根目录下、jar包里面，这样可以把静态资源压缩到jar包中。cache-period 可以使得静态资源进行web cache.

### 请求如何映射到具体的Action中的方法？
**方案一**

基于xml配置映射，可以利用SimpleUrlHandlerMapping、BeanNameUrlHandlerMapping进行Url映射和拦截请求。
配置方法略。

**方案二：**

基于注解映射，可以使用DefaultAnnotationHandlerMapping。
```
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping">  </bean>   
```
但前面我们配置了<mvc:annotation-driven />，他会自动注册这个bean,就不须要我们显式的注册这个bean了。  
在action类上使用：\
@Controller\
@RequestMapping("/user")

### Model、ModelMap及ModelAndView之间的区别
SpringMVC在调用方法前会创建一个隐含的数据模型(Model)，作为模型数据的**存储容器**， 成为”隐含模型”。 
1. Model（org.springframework.ui.Model）
Model是一个接口，包含addAttribute方法，其实现类是ExtendedModelMap。
ExtendedModelMap继承了ModelMap类，ModelMap类实现了Map接口。

Model通过以下方法向页面传递参数：  
`Model addAttribute(String attributeName, Object attributeValue)`

2. ModelMap（org.springframework.ui.ModelMap）
Spring框架自动创建modelmap的实例，并作为controller方法的参数传入，用户无需自己创建对象。
ModelMap对象主要用于把controller方法处理的数据传递到jsp界面，在controller方法中把jsp界面需要的数据放到ModelMap对象中即可。它的作用类似request对象的setAttribute方法。通过以下方法向页面传递参数：  
`ModelMap addAttribute(String attributeName, Object attributeValue)`

在视图层通过request找到ModelMap中的数据。
Modelmap本身不能设置页面跳转的url，可以通过controller方法的返回值来设置跳转的url。

3. ModelAndView（org.springframework.web.servlet.ModelAndView）
ModelAndView对象有两个作用：
   1. 设置url地址（这也是ModelAndView和ModelMap的主要区别）。
   2. 把controller方法中处理的数据传到jsp页面，在controller方法中把jsp界面需要的数据放到ModelAndView对象中即可。然后return mv。

它的作用类似request对象的setAttribute方法。通过以下方法向页面传递参数：

`ModelAndView addObject(String attributeName, Object attributeValue)`

在界面上可以通过el变量方式${key}来获取ModelAndView中的数据。
可通过以下方法设置视图：

`void setViewName(String viewName)`

controller方法的返回值如果是ModelAndView，则其即包含模型数据信息，又包含视图信息，这样SpringMVC将使用包含的视图对模型数据进行渲染，**可以简单地将模型数据看成一个Map<String, Object>对象**。

## 第5章 数据绑定和表单标签库
数据绑定是将用户输入绑定到领域模型的一种特性。有了数据绑定，类型总是为String的HTTP请求参数，可用于填充不同类型的对象属性。

### 5.1 数据绑定概览
基于HTTP的特性，所有HTTP请求参数的类型均为字符串。在之前的项目中，save-product方法中需要先解析Product Form中的price属性，是因为它是一个String，却需要用BigDecimal来填充Product的price属性。有了数据绑定，就可以用下面的代码取代saveProduct方法部分。
```java
@RequestMapping(value="save-product")
public String saveProduct(Product product, Model model)
```
有了数据绑定，就不再需要ProductForm类，也不需要解析Product对象的price属性了。

- 数据绑定的另一个好处是：当输入验证失败时，它会重新生成一个HTML表单。手工编写HTML代码时，必须记着用户之前输入的值，重新填充输入字段。有了Spring的数据绑定和表单标签库后，它们会替你完成这些工作。

### 5.2 表单标签库
表单标签库中包含了可以用在JSP页面中渲染HTML元素的标签。为了使用这些标签，必须在JSP页面开头处声明taglib指令
```
<%@taglib prefix="form"
    uri = "http://www.springframework.org/tags/form" %>
```

标签 | 描述
---|---
form | 渲染表单元素
input | 渲染<input type="text"/>元素
password | 渲染<input type="password"/>元素
hidden | 渲染<input type = "hidden"/>元素
textarea | 渲染textarea元素
checkbox | 渲染一个<input type = "checkbox"/>元素
checkboxes | 渲染多个<input type = "checkbox"/>元素
radiobutton | 渲染一个<input type = "radio"/>元素
radiobuttons | 渲染多个<input type="radio"/>元素
select | 渲染一个选择元素
option | 渲染一个可选元素
options | 渲染一个可选元素列表
errors | 在span元素中渲染字段错误

#### 5.2.1 表单标签
表单标签用于渲染HTML表单。要使用渲染一个表单字段的其他任何标签，必须要有一个form标签。

属性 | 描述 
--- | ---
acceptCharset | 定义服务器接受的字符编码列表
commandName | 暴露表单对象之模型属性的名称。默认问command
cssClass | 定义要应用到被渲染form元素的CSS类
cssStyle | 定义要应用到渲染form元素的CSS样式
htmlEscape | 接受true或false
modelAttribute | 暴露表单支持对象的模型属性名称，默认为command

这个表没有包含HTML属性，如method和action。\
commandName属性或是其中最重要的属性，因为它定义了模型属性的名称，其中包含了一个表单支持对象，其属性将用于填充所生成的表单。如果该属性存在，则必须在返回包含该表单的视图的请求处理方法中添加相应的模型属性。

```
BookAddForm.jsp
<form: form commandName = "book" action = "save-book" method = "post">
    ...
</form:form>

Java
@RequestMapping(value = "/input-book")
public String inputBook(Model nodel) {
    ...
    model.addAttribute("book", new Book());
    return "BookAddForm";
}
```

#### 5.2.2 input标签
input标签渲染<input type="text"/>元素。
属性 | 描述
--- | ---
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
htmlEscape | 接受true或者false，表示是否应该对被选入的值进行HTML转义
path | 要绑定的属性路径

这个标签最重要的属性是path，它将这个输入字段绑定到表单支持对象的一个属性。\
例如，如果随附`<form/>`标签的commandName属性值为book，并且input标签的path属性值为isbn，那么input标签将被绑定到Book对象的isbn属性。

示例：\
`<form:input id="isbn" path="isbn" cssErrorClass="errorBox"/>`\
它将会被渲染成如下<input/>元素\
`<input type="text" id = "isbn" name = "isbn">`\
cssErrorClass 属性不起作用，除非isbn属性中有输入验证错误，并且采用同一个表单重新显示用户输入，在这种情况下，input标签就会被渲染成下面这个input元素。\
`<input type="text" id="isbn" name = "isbn" class="errorBox"/>`\
input标签也可以绑定到嵌套对象的属性，例如
`<form:input path="category.id"/>`

#### 5.2.3 password 标签
password 标签显<input type="password"/>元素。
属性 | 描述
--- | ---
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被选入input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
htmlEscape | 接受true或者法拉瑟，表示是否应该对被选入的值进行HTML转义
path | 要绑定的属性路径
showPassword | 表示应该显示或遮盖密码，默认为false

`<form:password id="pwd" path="password" cssClass="normal"/>`\

#### 5.2.4 hidden标签
hidden标签渲染<input type="hidden"/>元素。
属性 | 描述
--- | ---
htmlEscape | j接受true或false，表示是否应用于被渲染的值进行HTML转义
path | 要绑定的属性路径

hidden标签与input标签相似，只不过没有可视的外观，因此不支持cssClass和cssStyle属性。

示例：
<form:hidden path="productId"/>

#### 5.2.5 textarea标签
textarea标签渲染一个HTML的textarea元素。Textarea实际上就是支持多行输入的一个input元素。
属性  | 描述
--- | --- 
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
html | 接受true或false，表示是否应该对被渲染的值进行HTML转义
path | 要绑定的属性路径
<form:textarea path="note" tabindex="4" row="5" cols="80"/>

#### 5.2.6 checkbox 标签
checkbox标签渲染<input type = "checkbox"/>元素。
属性 | 描述
--- | ---
cssClass | 定义要应用到被渲染input元素的css类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
htmlEscape | 接受true或者法拉瑟，表示是否应该对被渲染的（多个）值进行HTML转义
label | 要作为标签用于被渲染复选框的值
path | 要绑定的路径
示例：\
`<form:checkbox path="outofStock" value="out of Stock"/>`

#### 5.2.7 radiobutton标签
radiobutton 标签渲染<input type = "radio"/>元素
属性 | 描述
--- | ---
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
html | 要作为标签用于被渲染复选框的值
path | 要绑定的属性路径

5.2.8 checkboxes 标签
checkboxes 标签渲染多个<input type="checkbox"/>元素。
属性 | 描述
--- | ---
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
delimiter | 定义两个input元素之间的分隔符，默认没有分隔符
element | 给每个被渲染的input元素都定义一个HTML元素，默认为"span"
htmlEscape | 接受true或者false，表示是否应该对被渲染的（多个）值进行HTML转义
items | 用于生成input元素的对象的Collection、Map或者Array
itemLabel | item属性中定义的Collection、Map或者Array中的对象属性，为每个input元素提供标签
itemValue | item属性中定义的Collection、Map 或者Array 中的对象属性为每个input元素提供值
path | 要绑定的属性路径

#### 5.2.9 radiobuttons标签
radiobuttons标签渲染多个<input type = "radio"/>元素。
下面的radiobuttons标签将model属性categoryList的内容渲染为单选按钮。每次只能选择一个单选按钮。\
`<form:radiobuttons path="category" items="${categoryList}"/>

属性 | 描述
---- | ----
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染
delimiter | 定义两个input元素之间的分隔符，默认没有分隔符
element | 给每个被渲染的input元素都定义一个HTML元素，默认为"span"
htmlEscape | 接受true或者false， 表示是否应该对被渲染的值进行HTML转义
items | 用于声明input 元素的对象的Collection、Map或者Array中的对象属性，为每个input元素提供标签
itemValue | item属性中定义的Collection、Map或者Array中的对象属性，为每个input元素提供值
path | 要绑定的属性路径

#### 5.2.10 select标签
select标签渲染一个HTML的select元素。被渲染元素的选项可能来自赋予其items属性的一个Collection、Map、Array，或者来自一个嵌套的option或者options标签。
属性 | 描述
---- | ----
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
htmlEscape | 几首true或者false，表示是否应该对被渲染的（多个）值进行HTML转义
items | 用于生成input元素的对象的Collection、Map或者Array中的对象属性，为每个input元素提供标签
itemValue | item属性中定义的Collection、Map或者Array中的对象属性，为每个input元素提供值
path | 要绑定的属性路径

#### 5.2.11 option标签
option标签渲染select元素中使用的一个HTML的option元素。
属性 | 描述
---- | ----
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass属性值
htmlEscape | 接受true或者false，表示是否应该对被渲染的（多个）值进行HTMl转义

示例
```jsp
<form:select id = "category" path="category.id"
        item = "${categories}" itemLabel = "name"
        itemValue="id">
    <option value = "0"> -- please select-- </option>
</form:select>
```
渲染一个select元素，其选项来自model属性categories，以及option标签。

#### 5.2.12 options标签
options 标签生成一个HTML的option元素列表。
属性 | 描述
---- | ----
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
cssErrorClass | 定义要应用到被渲染input元素的CSS类，如果bound属性中包含错误，则覆盖cssClass的属性值
htmlEscape | 接受true或者false，表示是否应该对被渲染的（多个）值进行HTML转义
items | 用于生成input元素的Collection、Map或者Array
itemLabel | item属性中定义的Collection、Map或者Array中的对象属性，为每个input元素提供标签
itemValue | item属性中定义的Collection、Map或者Array中的对象属性，为每个input元素提供值

#### 5.2.13 errors标签
errors标签渲染一个或者多个HTML的审判元素，每个span元素中都包含一个字段错误。
这个标签可以用于显示一个特定的字段错误，或者所有字段错误。
属性 | 描述
---- | ----
cssClass | 定义要应用到被渲染input元素的CSS类
cssStyle | 定义要应用到被渲染input元素的CSS样式
Delimiter | 分隔多个错误消息的分隔符
element | 定义一个包含错误消息的HTML元素
htmlEscape | 接受true或者false，表示是否应该对被渲染的（多个）值进行HTML转义
path | 要绑定的错误对象路径

## 第6章 转换器和格式化
如果想让Spring使用不同的日期样式，就需要一个Converter（转换器）或者 Formatter（格式化）来协助Spring完成。
两者均可用于将一种对象类型转换成另一种对象类型。  
==Converter是通用原件，可以在应用程序的任意层中使用，而Formatter则是专门为Web层设计的==

### 6.1 Converter转换器
Spring的Converter是可以将一种类型转换成另一种类型。为了创建Converter，，必须编写实现org.springframework.core.convert.Converter接口的一个Java类。  
`public interface Converter<S,T>`  
这里的S表示源类型，T表示目标类型。例如，为了创建一个可以将Long转换成Date的Converter，要这样声明：  
`public class MyConverter implements Converter<Long, LocalDate>{}`  
再类实体中需要编写一个来自Converter接口的Convert方法实现。  
`T convert(S source)`   

下面展示一个只用于任意日期样式的Converter：
```
package converter;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import org.springframework.core.convert.Converter;

public class StringToLocalDateConverter implements Converter<String, LocalDate>{
    private String datePattern;
    public StringToLocalDateConverter(String datePattern){
        this.datePattern = datePattern;
    }
    
    @Override
    public LocalDate convert(String s) {
        try{
            return LocalDate.parse(s, DateTimeFormatter.ofPattern(datePattern));
        }catch (DatetimeParseException e){
            // the error message will be displayed in <form:errors>
            throw new IllegalArgumentException{
                "invalid date format. Please use this pattern\""+datePattern + "\"");
            }
        }
    }
}
```
为了使用SpringMVC应用程序中定制的Converter，需要再SpringMVC配置文件中编写一个名为conversionService的bean。bean的类名必须为org.springframework.context.support.ConversionServiceFactoryBean。这个bean必须包含一个converters属性，它将列出要在应用程序中使用的所有定制Converter。  
例如：
```html
<bean id = "conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name = "converters">
        <list>
            <bean class = "converter.StringToLocalDateConverter">
                <constructor-arg type="java.lang.String" value="MM-dd-yyyy"/>
            </bean>
        </list>
    </property>
</bean>
```
随后要给annotation-driven元素的conversion-Service属性赋值bean名称（本例中是conversionService），如下所示   
`<mvc:annotation-driven conversion-service="conversionService"`  

### 6.2 Formatter
Formatter将一种类型转换成另一种类型。Formatter源类型必须是一个String，而Converter则适用于任意的源类型。为了转换Spring MVC应用程序表单中的用户输入，始终应该选择Formatter而不是Converter。
创建Formatter，要编写一个实现org.springframework.format.Formatter接口的Java类。  
`public interface Formatter<T>`

这里的T表示输入字符串要转换的目标类型。该接口有parse和print两个方法，所有实现都必须要覆盖它们。  
` T parse(String text, java.util.Locale locale`  
`String print(T object, java.util.Locale locale)`

parse方法利用指定的Locale将一个String解析成目标类型。  
print方法与之相反，它返回目标对象的字符串表示法。

在SpringMVC应用程序中使用Formatter，需要利用名为conversionService的bean对它进行注册。  
bean的类名为
`org.springframework.format.support.format.support.FormattingConversionServiceFactoryBean`  
这个bean可以用一个formatters属性注册formatter，用一个converters属性注册converter。

### 6.3 用Register注册Formatter
注册Formatter的另一种方法是使用Registrar。
```
package formatter;
import org.springframework.format.FormatterRegistrar;
import org.springframework.format.FormatterRegistry;
public class MyFormatterRegistrar implements FormatterRegistrar{
    private String dataPattern;
    public MyFormatterRegistrar(String datePattern){
        this.datePattern = datePattern;
    }
    
    @Override
    public void registerFormatters(FormatterRegistry registry) {
        registry.addFormatter(new LocalDateFormatter(datePattern));
        // register more formatters here
    }
}
```

有Registrar就无需再Spring MVC配置文件中注册，只需注册Register即可。

### 6.4 选择Converter，还是Formatter
- Converter是一般工具，可以将一种类型转换成另一种类型。例如，将String转换成LocalDate，或者将Long转换成LocalDate。Converter既可以用在Web层，也可以用在其他层中。
- Formatter只能将String转换成另一种Java类型。例如，将String转换成LocalDate，但它不能将Long转换成LocalDate。因此，Formatter适用于Web层，为此，再SpringMVC应用程序中，选择Formatter比选择Converter更合适。


## 第7章 验证器
输入验证时Spring处理的更重要Web开发任务之一。在SpringMVC中，有两种方式可以验证输入，即利用Spring自带的验证框架。或者利用JSR 303实现。
分别是`spring-validator`和`jsr303-validator`

### 7.1 验证概览
Converter和Formatter作用于字段级。在MVC应用程序中，它们将String转换或格式化成另一种Java类型，如java.time.LocalDate。验证器则作用于对象级。它决定某一个对象中的所有字段是否均是有效的，以及是否遵循某些规则。一个典型的Spring MVC 应用会同时应用到formatters/converters和validators。  
如果一个应用程序既使用了Formatter，又有validator（验证器），那么，应用中的时间顺序：在调用Controller期间，将会有一个或者多个Formatter，试图将输入字符串转换成domain对象中的field值，一旦格式化成功，验证器就会介入。   

### 7.2 Spring验证器
从一开始，Spring就设计了输入验证，甚至早于JSR 303（Java验证规范）。因此Spring的validation框架至今都很普遍，尽管对于新项目，一般建议选用JSR 303验证器。  
创建SPringle验证其，要实现org.springframework,validation.Validator接口，其中有supports和validate两个方法。
```Java
package org.springframework.validation;
public interface validator{
    boolean supports(Class<?> clazz)
    void validate(Object target, Errors errors);
}
```
如果验证其可以处理指定的Class，support方法将返回true，validate方法会验证目标对象，并将验证错误填入Errors对象。  
Errors对象是org.springframework.validation.Errors接口的一个实例。Errors对象包含了一系列FieldError（字段错误）和ObjectError（对象错误）对象。FieldError表示与被验证对象中的某个属性相关的一个错误。

编写验证器时，不需要直接创建Error对象。
给Errors对象添加错误最容易的方法是：在Errors对象上调用一个reject或者rejectValue方法。调用reject，会往FieldError中添加一个ObjectError和rejectValue。

```java
void reject(String errorCode)
void reject(String errorCode, String errorCode)
void rejectValue(String field, String errorCode)
void rejectValue(String field, String errorCode, String defaultMessage)
```

大多数时候，只给reject或者rejectValue方法传入以一个错误码，Spring就会在属性文件中查找错误码，获得相应的错误消息。还可以传入一个默认消息，当没有找到指定的错误码时，就会使用默认消息。
Errors对象中的错误消息，可以利用表单标签库的Errors标签显示在HTML页面中。错误消息可以通过Spring支持的国际化特征性本地化。

### 7.3 ValidationUtils 类
org.springframework.validation.validationUtils类是一个工具，有助于编写Spring验证器。
```java
//以前写验证
if (firstName == null || firstName.isEmpty()) {
    errors.rejectValue("price");
}
```
而是可以利用ValidationUtils类的rejectIfEmpty方法：  
`ValidationUtils.rejectIfEmpty("price");`  

或者下面这样的代码：   
```java
if(firstName = null || firstName.trim().isEmpty()) {
    errors.rejectValue("price");
}
```
可以编写为:   
`ValidationUtils.rejectIfEmptyOrWhitespace("price");`  

rejectIfEmpty和rejectIfEmptyOrWhitespace方法的方法重载：
```
public static void rejectIfEmpty(Errors, String field, String errorCode)

public static void rejectIfEmpty(Errors, String field, String field,
        String errorCode, Object[] errorArgs, String defaultMessage)
...
```

### 7.4 JSR 303验证
JSR303 不需要编写验证器，但要利用JSR 303 注解类型嵌入约束。
属性 | 描述 | 范例 
---- | ---- | ----
@AssertFalse | 应用于Boolean属性， 该属性值必须为False | @AssertFalse boolean hasClidren;
@AssertTrue | 应用于Boolean属性，改制必须为True | @AssertTrue boolean isEmpty;
@DecimalMax | 该属性值必须为小于或者等于指定值的小数 | @DecimalMax("1.1") BigDecimal price;
@DecimalMin | 该属性值必须为大于或等于指定值的小数 | @DecimalMin("0.04") BigDecimal price;
@Digits | 该属性值必须在指定范围内。integer属性定义该数值的最大整数部分，fraction属性定义该数值的最大小数部分 | @Digits(integer = 5, fraction = 2) BigDecimal price;
@Future | 该属性值必须是未来的一个日期 | @Future Date shippingDate;
@Max | 该属性值必须是一个小于或者等于指定值的整数 | @Max(150) int age;
@Min | 该属性值必须是一个大于或者等于指定值的整数 | @Min(150) int age;
@NotNull | 该属性不能为Null | @NotNull String testString;
@Null | 该值必须为Null | @Null String testString;
@Past | 该属性值必须是过去一个日期 | @Past Date birthDate;
@Pattern | 该属性值必须与指定的常规表达式相匹配 | @Pattern(regext="\\d{3}" String areaCode);
@Size | 该属性值必须在指定范围内 | Size(min = 2, Max = 140) String description;

像使用Spring验证器一样，可以在属性文件中以下列格式来使用property键，覆盖来自JSR 303验证器的错误消息:  
`constraint.object.property`  
例如，为了覆盖以@Size注解约束的Product对象的name属性，可以在属性文件中使用下面这个键：  
`Size.product.name`
为了覆盖以@Past注解约束的Product对象的productionDate属性，可以在属性文件中使用下面这个键：  
`Past.product.productionDate`


## 第8章 表达式语言
### 8.1 表达式语言的语法
EL表达式以 ${开头，并以}结束。EL表达式的结构如下：
${expression}
#{expression}

例如，表达式x+y,可以写成:${x+y}或#{x+y}  
${exp}和#{exp}结构都由EL引擎以相同的方式进行计算，然而，当EL未被用作独立引擎而是使用注入JSF或JSP的底层技术时，该技术都可以不同得解释构造。  
例如，在JSF中，${exp}结构用于延迟计算（即表达式直到系统需要它的值的时候，才进行计算）。另一方面，立即计算的表达式，会在JSP页面编译时同时编译，并在执行JSP页面时被执行。在JSP2.1 和更高版本中，#{exp}表达式只能在接受延迟表达式的标签属性中使用。

两个表达式可以连接在一起，对于一系列的表达式，它们的取值将是从左到右进行，计算结果的类型为String，并且连接在一起，假如a+b等于8，c+d等于10，那么两个表达式的计算结构将是810:  
${a+b}${c+d}
表达式${a+b}and${c+d}的取值结果则是8and10.
**如果在定制标签的属性值中使用EL表达式，那么该表达式的取值结果字符串将会强制变成该属性需要的类型**：  
<my:tag someAttribute="$(expression)"/>  
像${这样的字符顺序就是表示EL表达式的开头。如果需要的指示文本${,则需要在之前加转义字符\${

#### 8.1.1 关键字
以下关键字不能作为标识符：
and eq gt true instanceof
or ne le false empty
not lt ge null div mod

#### 8.1.2 []和.运算符
EL表达式可以返回任意类型的值。如果EL表达式的结果是一个带有属性的对象，则可以利用[]或者.运算符来访问该属性。[]和.运算符类似；[]时比较规范的形式,.运算符时比较快捷的形式。  
为了访问对象的属性，可以使用下列形式：  
```
${object["propertyName"]}
@{object.propertyName}
```
但是，如果propertyName不是有效的Java变量名，只能使用[]运算符。例如，下面这俩表达式就可以访问隐式对象标题中的HTTP标题host：
```
${header["host"]}
${header.host}
```
但是，要想访问accpet-language 标题，只能使用[]运算符，因为accept-language不是一个合法的Java变量名。如果用.运算符，将会导致异常。  
如果对象的属性碰巧返回带有拎一个对象，也可用[]或.元素安抚来访问第二个对象的属性。

### 8.1.3 取值规则
EL表达式的取值时从左到右进行的。对于expr-a[expr-b]形式的表达式，其EL表达式的取值方法如下：
1. 先计算expr-a得到value-a。
2. 如果value-a为null,则返回null
3. 然后计算expr-b得到value-b。
4. 如果value-b为null，则返回null
5. 如果value-a为java.util.Map,则会查看value-b是否为Map中的一个key。若是，则返回value-a.get(value-b),若不是，则返回null。
6. 如果value-a为 java.util.List,或者假如它是一个array,则要进行一下处理：  
   1. 强制value-b为int，如果强制失败，则抛出异常。
   2. 如果value-a.get(value-b)抛出IndexOutOfBoundsException，或者加入Array.get(value-a,value-b)抛出ArrayIndexOutOfBoundsException,则返回null。
   3. 否则，若value-a是个List，则返回value-a.get(value-b); 若value-a是个array，则返回Array.get(value-a,value-b)。
7. 如果value-a不是一个Map、List或者array，那么value-a必须是一个JavaBean.在这种情况下，必须强制value-b为String，如果value-b是value-a的一个可读属性，则要调用该属性的getter方法，从中返回值。如果getter方法抛出异常，该表达式就是无效的，否则，该表达式有效。


## 8.2 访问JavaBean
利用.或[]运算符，都可以访问bean的属性，其结构如下：
```
${beanName["propertyName"]}
${beanName.propertyName}
```
例如，访问myBean的secret属性，可以使用一下表达式：  
`${myBean.secret}`

如果该属性是一个带属性的对象，那么同样也可以利用.或者[]运算符来访问第二个对象的该属性。

## 8.3 EL隐式对象
在JSP页面中，可以利用JSP脚本来访问JSP隐式对象。但是，在免脚本的JSP页面中，则不可能访问这些隐式对象。EL允许通过提供一组它自己的隐式对象来访问不同的对象。
对象 | 描述 
---- | ----
pageContext | 这是当前JSP的javax.servlet.jsp.PageContext
initParam | 这是一个包含所有环境初始化参数并用参数名作为key的Map
param | 这是一个包含所有请求参数并用参数名作为key的Map。每个key的值就是指定名称的第一个参数。因此，如果两个请求参数同名，则只有一个能够利用param获取值。要访问同盟参数的所有参数值可以用params代替
paramValues | 这是一个包含所有请求参数并用参数名作为key的Map。每个key的值就是一个字符串数组，其中包含了指定参数名称的所有参数值。就算该参数只有一个值，它也仍然会返回一个带有一个元素的数组。
header | 这是一个包含请求标题并用标题名作为key的Map，每个key的值就是指定标题名称的第一个标题。换句话说，如果一个标题的值不止一个，则只返回第一个值。要想获得多个值的标题，得用headerValues对象代替
headerValues | 这个是一个包含请求标题并用标题名作为key的Map。每个key的值就是一个字符串数组，其中包含了指定标题名称的所有参数值。就算该标题只有一个值，它也仍然会返回一个带有一个元素的数组
cookie | 这是一个包含了当前请求对象中所有Cookie对象的Map。Cookie名称就是key名称，冰球每个key都映射到一个Cookie对象
applicationScope | 这是一个包含了ServletConetext对象中所有属性的Map，并用属性名称作为key
sessionScope | 这是一个包含了HttpSession对象中所有对象的Map，并用属性名称作为key
requestScope | 这是一个Map，其中包含了当前HttpServletRequest对象中的所有属性，并用属性名称作为key
pageScope | 这是一个Map，其中包含了全页面范围内的所有属性。属性名称就是Map的key

### 8.3.1 pageContext
pageContext对象表示当前JSP页面的javax.servlet.jsp.PageContext。它包含了所有其他的JSP隐式对象。
对象 | EL中的类型
---- | ----------
reques | javax.servlet.http.HttpServletRequest
response | javax.servlet.http.HttpServletResponse
out | javax.servlet.jsp.JspWriter
session | javax.servlet.http.HttpSession
application | javax.servlet.ServletContext
config | javax.servlet.ServletConfig
PageContext | javax.servlet.jsp.PageContext
page | javax.servlet.jsp.HttpJspPage
exception | java.lang.Throwable

例如，可以利用以下任意一个表达式来获取当前的ServletRequest:
${page}
