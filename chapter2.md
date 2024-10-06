# Servlets

## Servlet API Overview

The Servlet API is a key component of the Java EE (Enterprise Edition) platform, providing a framework for building web applications. It defines the classes and interfaces that developers use to write servlets, which are Java programs that extend the capabilities of a server. Servlets can respond to requests from web clients, typically browsers, and generate dynamic content.

## Key Components of the Servlet API

| Component | Description |
|-----------|-------------|
| **Servlet Interface** | The Servlet interface is the central abstraction in the Servlet API. All servlets must implement this interface, either directly or, more commonly, by extending classes that implement it. |
| **HttpServlet Class** | The HttpServlet class is a convenience class that extends GenericServlet and implements the Servlet interface. It is designed to handle HTTP-specific services. |
| **ServletRequest Interface** | The ServletRequest interface provides methods for accessing request information, such as parameters, attributes, and input streams. |
| **HttpServletRequest Interface** | Extends ServletRequest and provides additional methods for accessing HTTP-specific request information. |
| **HttpServletResponse Interface** | Extends ServletResponse and provides additional methods for generating HTTP-specific responses. |
| **ServletConfig Interface** | The ServletConfig interface provides servlet configuration information, such as initialization parameters and the servlet context. |
| **ServletContext Interface** | The ServletContext interface provides a way to interact with the servlet container. It allows servlets to access resources, share data, and log information. |

## Servlet

Servlet technology is used to create a web application (resides at server side and generates a dynamic web page).
Servlet technology is robust and scalable because of java language. Before Servlet, CGI (Common Gateway Interface) scripting language was common as a server-side programming language. However, there were many disadvantages to this technology. We have discussed these disadvantages below.

There are many interfaces and classes in the Servlet API such as Servlet, GenericServlet, HttpServlet, ServletRequest, ServletResponse, etc.

Servlet can be described in many ways, depending on the context.

- Servlet is a technology which is used to create a web application.
- Servlet is an API that provides many interfaces and classes including documentation.
- Servlet is an interface that must be implemented for creating any Servlet.
- Servlet is a class that extends the capabilities of the servers and responds to the incoming requests. It can respond to any requests.
- Servlet is a web component that is deployed on the server to create a dynamic web page.

![image](https://github.com/user-attachments/assets/78e2fd51-4b12-4b79-92f7-a3184f6d5f37)

Other tasks that a servlet can do effectively are:

- Can easily manage/control the application flow.
- Suitable to implement business logic.
- Can effectively balance the load on the server side.
- Easily generate dynamic web content.
- Handle HTTP Request and Response
- Also act as an interceptor or filter for a specific group of requests.

### Types of Servlet

- Generic Servlets

  These are those servlets that provide functionality for implementing a servlet. It is a generic class from which all the customizable servlets are derived. It is protocol-independent and provides support for HTTP, FTP, and SMTP protocols. The class used is ‘javax.servlet.Servlet’ and it only has 2 methods – init() to initialize & allocate memory to the servlet and destroy() to deallocate the servlet.
  
- HTTP Servlets

  These are protocol dependent servlets, that provides support for HTTP request and response. It is typically used to create web apps. And has two of the most used methods – doGET() and doPOST() each serving their own purpose.

There are three potential ways in which we can employ to create a servlet:

- Implementing Servlet Interface
- Extending Generic Servlet
- Extending HTTP Servlet

### Servlet Container

Servlet container, also known as Servlet engine, is an integrated set of objects that provide a run time environment for Java Servlet components. 
In simple words, it is a system that manages Java Servlet components on top of the Web server to handle the Web client requests. 

Services provided by the Servlet container:

- Network Services: Loads a Servlet class. The loading may be from a local file system, a remote file system or other network services. The Servlet container provides the network services over which the request and response are sent.
- Decode and Encode MIME-based messages: Provides the service of decoding and encoding MIME-based messages.
- Manage Servlet container: Manages the lifecycle of a Servlet.
- Resource management: Manages the static and dynamic resources, such as HTML files, Servlets, and JSP pages.
- Security Service: Handles authorization and authentication of resource access.
- Session Management: Maintains a session by appending a session ID to the URL path.

### Life Cycle of a Servlet

The entire life cycle of a Servlet is managed by the Servlet container which uses the javax.servlet.Servlet interface to understand the Servlet object and manage it. So, before creating a Servlet object, let’s first understand the life cycle of the Servlet object which is actually understanding how the Servlet container manages the Servlet object.

Stages of the Servlet Life Cycle: The Servlet life cycle mainly goes through four stages,

- Loading a Servlet.
- Initializing the Servlet.
- Request handling.
- Destroying the Servlet.

![image](https://github.com/user-attachments/assets/ccb1a798-cfeb-4469-bc32-ac36e84be440)

Let’s look at each of these stages in details:

1. Loading a Servlet: The first stage of the Servlet lifecycle involves loading and initializing the Servlet by the Servlet container. The Web container or Servlet Container can load the Servlet at either of the following two stages:

   - Initializing the context, on configuring the Servlet with a zero or positive integer value.
   - If the Servlet is not preceding stage, it may delay the loading process until the Web container determines that this Servlet is needed to service a request.

     The Servlet container performs two operations in this stage:

     - Loading: Loads the Servlet class.
     - Instantiation: Creates an instance of the Servlet. To create a new instance of the Servlet, the container uses the no-argument constructor.

2. Initializing a Servlet: After the Servlet is instantiated successfully, the Servlet container initializes the instantiated Servlet object. The container initializes the Servlet object by invoking the Servlet.init(ServletConfig) method which accepts ServletConfig object reference as parameter.

   The Servlet container invokes the Servlet.init(ServletConfig) method only once, immediately after the Servlet.init(ServletConfig) object is instantiated successfully. This method is used to initialize the resources, such as JDBC datasource.

   Now, if the Servlet fails to initialize, then it informs the Servlet container by throwing the ServletException or UnavailableException.

3. Handling request: After initialization, the Servlet instance is ready to serve the client requests. The Servlet container performs the following operations when the Servlet instance is located to service a request :

   - It creates the ServletRequest and ServletResponse objects. In this case, if this is a HTTP request, then the Web container creates HttpServletRequest and HttpServletResponse objects which are subtypes of the ServletRequest and ServletResponse objects respectively.
   - After creating the request and response objects it invokes the Servlet.service(ServletRequest, ServletResponse) method by passing the request and response objects.

   The service() method while processing the request may throw the ServletException or UnavailableException or IOException.

4. Destroying a Servlet: When a Servlet container decides to destroy the Servlet, it performs the following operations,

   - It allows all the threads currently running in the service method of the Servlet instance to complete their jobs and get released.
   - After currently running threads have completed their jobs, the Servlet container calls the destroy() method on the Servlet instance.

   After the destroy() method is executed, the Servlet container releases all the references of this Servlet instance so that it becomes eligible for garbage collection.

### Servlet Life Cycle Methods

There are three life cycle methods of a Servlet :

- init()
- service()
- destroy()

![image](https://github.com/user-attachments/assets/68480099-87f3-423e-94db-a4687f875744)

Let’s look at each of these methods in details:

1. init() method: The Servlet.init() method is called by the Servlet container to indicate that this Servlet instance is instantiated successfully and is about to put into service.

  ![image](https://github.com/user-attachments/assets/bec06618-4f55-45db-87ae-651df6d38eed)

2. service() method: The service() method of the Servlet is invoked to inform the Servlet about the client requests.

   - This method uses ServletRequest object to collect the data requested by the client.
   - This method uses ServletResponse object to generate the output content.

   ![image](https://github.com/user-attachments/assets/feb974a3-3760-4635-b7c0-c9da8db6ea18)

3. destroy() method: The destroy() method runs only once during the lifetime of a Servlet and signals the end of the Servlet instance.

   ![image](https://github.com/user-attachments/assets/ab46598a-0c47-46d4-a227-d0567eb782f1)

   As soon as the destroy() method is activated, the Servlet container releases the Servlet instance.

### Deployment Descriptor

The deployment descriptor is a configuration file for a web application deployed to a servlet container (e.g., Apache Tomcat). It is an XML file named web.xml and is located in the WEB-INF directory of the web application. The deployment descriptor provides configuration and deployment information for the web application, such as servlet definitions, servlet mappings, context parameters, session configuration, and security settings.

The web.xml file follows a specific XML schema defined by the Servlet specification. Below is a typical structure of a deployment descriptor:

```
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <!-- Servlet Definitions -->
    <servlet>
        <servlet-name>HelloWorldServlet</servlet-name>
        <servlet-class>com.example.HelloWorldServlet</servlet-class>
    </servlet>

    <!-- Servlet Mappings -->
    <servlet-mapping>
        <servlet-name>HelloWorldServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>

    <!-- Context Parameters -->
    <context-param>
        <param-name>configParam</param-name>
        <param-value>configValue</param-value>
    </context-param>

    <!-- Session Configuration -->
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>

    <!-- Welcome File List -->
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <!-- Error Pages -->
    <error-page>
        <error-code>404</error-code>
        <location>/error404.html</location>
    </error-page>
    <error-page>
        <exception-type>java.lang.Throwable</exception-type>
        <location>/error.jsp</location>
    </error-page>

    <!-- Security Constraints -->
    <security-constraint>
        <web-resource-collection>
            <web-resource-name>Protected Area</web-resource-name>
            <url-pattern>/protected/*</url-pattern>
        </web-resource-collection>
        <auth-constraint>
            <role-name>admin</role-name>
        </auth-constraint>
    </security-constraint>

    <!-- Login Configuration -->
    <login-config>
        <auth-method>FORM</auth-method>
        <form-login-config>
            <form-login-page>/login.html</form-login-page>
            <form-error-page>/login-error.html</form-error-page>
        </form-login-config>
    </login-config>

    <!-- Security Roles -->
    <security-role>
        <role-name>admin</role-name>
    </security-role>

</web-app>
```

The web.xml file is an essential part of Java web application deployment, providing a structured and standardized way to configure various aspects of the application.
