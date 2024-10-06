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
