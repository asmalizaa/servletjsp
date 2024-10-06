# JavaServer Pages (JSP)

JavaServer Pages (JSP) is a technology used to create dynamic web content by embedding Java code into HTML pages. Understanding the architecture of JSP is crucial for building robust and scalable web applications. This guide will walk you through the key components and workflow of JSP architecture.

- It is a server-side technology.
- It is used for creating web application.
- It is used to create dynamic web content.
- In this JSP tags are used to insert JAVA code into HTML pages.
- It is an advanced version of Servlet Technology.
- It is a Web based technology helps us to create dynamic and platform-independent web pages.
- In this, Java code can be inserted in HTML/ XML pages or both.
- JSP is first converted into servlet by JSP container before processing the client’s request.

## Key Components of JSP

| Component | Description |
|-----------|-------------|
| **JSP Page** | The JSP file, which contains HTML tags, JSP tags, and Java code snippets. |
| **Servlet Container** | Also known as the web container, this is the runtime environment that manages the execution of JSP pages and servlets. |
| **JSP Engine** | A part of the servlet container, the JSP engine handles the conversion of JSP pages into servlets. |
| **Web Server** | The server that handles HTTP requests and responses, and interacts with the servlet container. |

## JSP Pages are more advantageous than servlet

- They are easy to maintain.
- No recompilation or redeployment is required.
- JSP has access to entire API of JAVA.
- JSP are extended version of Servlet.

### JSP API

The JSP API consists of two packages:

- javax.servlet.jsp
- javax.servlet.jsp.tagext

### javax.servlet.jsp package

The javax.servlet.jsp package has two interfaces and classes.The two interfaces are as follows:

- JspPage
- HttpJspPage

The classes are as follows:

- JspWriter
- PageContext
- JspFactory
- JspEngineInfo
- JspException
- JspError

### The JspPage interface

According to the JSP specification, all the generated servlet classes must implement the JspPage interface. It extends the Servlet interface. It provides two life cycle methods.

![image](https://github.com/user-attachments/assets/aa3a6956-7e14-4235-addb-75ebbe7b8692)

Methods of JspPage interface

1. public void jspInit(): It is invoked only once during the life cycle of the JSP when JSP page is requested firstly. It is used to perform initialization. It is same as the init() method of Servlet interface.
2. public void jspDestroy(): It is invoked only once during the life cycle of the JSP before the JSP page is destroyed. It can be used to perform some clean-up operation.

### The HttpJspPage interface

The HttpJspPage interface provides the one life cycle method of JSP. It extends the JspPage interface.

Method of HttpJspPage interface:

1. public void _jspService(): It is invoked each time when request for the JSP page comes to the container. It is used to process the request. The underscore _ signifies that you cannot override this method.

## Life Cycle of JSP

A Java Server Page life cycle is defined as the process that started with its creation which later translated to a servlet and afterward servlet lifecycle comes into play. This is how the process goes on until its destruction.

![image](https://github.com/user-attachments/assets/105bc121-d537-4138-953c-877e369a788b)

Following steps are involved in the JSP life cycle:  

- Translation of JSP page to Servlet
- Compilation of JSP page(Compilation of JSP into test.java)
- Classloading (test.java to test.class)
- Instantiation(Object of the generated Servlet is created)
- Initialization(jspInit() method is invoked by the container)
- Request processing(_jspService()is invoked by the container)
- JSP Cleanup (jspDestroy() method is invoked by the container)

### Translation of JSP page to Servlet

This is the first step of the JSP life cycle. This translation phase deals with the Syntactic correctness of JSP. Here test.jsp file is translated to test.java. 

- Compilation of JSP page: Here the generated java servlet file (test.java) is compiled to a class file (test.class).
- Classloading: Servlet class which has been loaded from the JSP source is now loaded into the container.
- Instantiation: Here an instance of the class is generated. The container manages one or more instances by providing responses to requests.
- Initialization: jspInit() method is called only once during the life cycle immediately after the generation of Servlet instance from JSP.
- Request processing: _jspService() method is used to serve the raised requests by JSP. It takes request and response objects as parameters. This method cannot be overridden.
- JSP Cleanup: In order to remove the JSP from use by the container or to destroy the method for servlets jspDestroy()method is used. This method is called once, if you need to perform any cleanup task like closing open files, releasing database connections jspDestroy() can be overridden.

## Difference between Servlet and JSP

Brief Introduction: Servlet technology is used to create a web application. A servlet is a Java class that is used to extend the capabilities of servers that host applications accessed by means of a request-response model. Servlets are mainly used to extend the applications hosted by web services. 

JSP is used to create web applications just like Servlet technology. A JSP is a text document that contains two types of text: static data and dynamic data. The static data can be expressed in any text-based format (like HTML, XML, SVG, and WML), and the dynamic content can be expressed by JSP elements. 

The difference between Servlet and JSP is as follows:

| Servlet | JSP |
|---------|-----|
| Servlet is a java code. | JSP is a HTML-based compilation code. |
| Writing code for servlet is harder than JSP as it is HTML in java. | JSP is easy to code as it is java in HTML. |
| Servlet plays a controller role in the, MVC approach. | JSP is the view in the MVC approach for showing output. |
| Servlet is faster than JSP. | JSP is slower than Servlet because the first step in the JSP lifecycle is the translation of JSP to java code and then compile. |
| Servlet can accept all protocol requests.	| JSP only accepts HTTP requests. |
| In Servlet, we can override the service() method. | In JSP, we cannot override its service() method. |
| In Servlet by default session management is not enabled, user have to enable it explicitly. | In JSP session management is automatically enabled. |
| In Servlet we have to implement everything like business logic and presentation logic in just one servlet file. | In JSP business logic is separated from presentation logic by using JavaBeansclient-side. |
| Modification in Servlet is a time-consuming compiling task because it includes reloading, recompiling, JavaBeans and restarting the server. | JSP modification is fast, just need to click the refresh button. |
| It does not have inbuilt implicit objects. | In JSP there are inbuilt implicit objects. | In JSP there are inbuilt implicit objects. |
| There is no method for running JavaScript on the client side in Servlet. | While running the JavaScript at the client side in JSP, client-side validation is used. |
| Packages are to be imported on the top of the program. | Packages can be imported into the JSP program (i.e, bottom, middleclient-side, or top ) |
| It can handle extensive data processing. | It cannot handle extensive data processing very efficiently. |
| The facility of writing custom tags is not present. | The facility of writing custom tags is present. |
| Servlets are hosted and executed on Web Servers. | Before the execution, JSP is compiled in Java Servlets and then it has a similar lifecycle as Servlets. |









