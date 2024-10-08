# Servlet Filters

Servlet filters are used for preprocessing Web application requests and postprocessing responses.

## Overview of Servlet Filters

When the servlet container calls a method in a servlet on behalf of the client, the HTTP request that the client sent is, by default, passed directly to the servlet. The response that the servlet generates is, by default, passed directly back to the client, with its content unmodified by the container. In this scenario, the servlet must process the request and generate as much of the response as the application requires.

But there are many cases in which some preprocessing of the request for servlets would be useful. In addition, it is sometimes useful to modify the response from a class of servlets. One example is encryption. A servlet, or a group of servlets in an application, may generate response data that is sensitive and should not go out over the network in clear-text form, especially when the connection has been made using a nonsecure protocol such as HTTP. A filter can encrypt the responses. Of course, in this case the client must be able to decrypt the responses.

A common scenario for a filter is one in which you want to apply preprocessing or postprocessing to requests or responses for a group of servlets, not just a single servlet. If you need to modify the request or response for just one servlet, there is no need to create a filter—just do what is required directly in the servlet itself.

Note that filters are not servlets. They do not implement and override HttpServlet methods such as doGet() or doPost(). Rather, a filter implements the methods of the javax.servlet.Filter interface. The methods are:

- init()
- destroy()
- doFilter()

## How the Servlet Container Invokes Filters

Figure below shows how the servlet container invokes filters. On the left is a scenario in which no filters are configured for the servlet being called. On the right, several filters (1, 2, ..., N) have been configured in a chain to be invoked by the container before the servlet is called and after it has responded. The web.xml file specifies which servlets cause the container to invoke the filters.

![image](https://github.com/user-attachments/assets/f3eb0a0c-9d69-4140-bf5a-a6ba8c3a26fb)

The order in which filters are invoked depends on the order in which they are configured in the web.xml file. The first filter in web.xml is the first one invoked during the request, and the last filter in web.xml is the first one invoked during the response. Note the reverse order during the response.

## Filtering of Forward or Include Targets

When a servlet is filtered, any servlets that are forwarded to or included from the filtered servlet are not filtered by default.

## Filter Examples

Here is the generic filter code. Assume that this generic filter is part of a package com.example.filter and set up a corresponding directory structure.

This is an elementary example of an empty (or "pass-through") filter and could be used as a template.

![image](https://github.com/user-attachments/assets/dc67f29c-a986-4c25-9ac1-fc4b154d5a23)

Save this code in a file called MyGenericFilter.java in the package directory. The numbered code notes refer to the following:

- This code declares a variable to save the filter configuration object.
- The doFilter() method contains the code that implements the filter.
- In the generic case, just call the filter chain.
- The init() method saves the filter configuration in a variable.
- The destroy() method can be overridden to accomplish any required finalization.

## Filter Code: HelloWorldFilter.java

This filter overrides the doFilter() method of the MyGenericFilter class above. It prints a message on the console when it is called on entrance, then adds a new attribute to the servlet request, then calls the filter chain. In this example there is no other filter in the chain, so the container passes the request directly to the servlet. Enter the following code in a file called HelloWorldFilter.java:

![image](https://github.com/user-attachments/assets/5c4920cf-8315-4507-be34-44d9d8a1a61b)

## JSP Code: filter.jsp

To keep the example simple, the "servlet" to process the filter output is written as a JSP page. Here it is:

![image](https://github.com/user-attachments/assets/fb3c541c-b123-434c-8015-1bfdbfe9b8ae)

The JSP page gets the new request attribute, hello, that the filter added, and prints its value on the console. Put the filter.jsp page in the root directory of the OC4J standalone default Web application, and make sure your console window is visible when you invoke filter.jsp from your browser.

## Setting Up

To test the filter examples in this chapter, use the OC4J standalone default Web application. Configure the filter in the web.xml file in the default Web application /WEB-INF directory (j2ee/home/default-web-app/WEB-INF, by default).

You will need the following entries in the <web-app> element:

![image](https://github.com/user-attachments/assets/427c658f-a0c6-405c-9094-ce2d7710eea0)

The <filter> element defines the name of the filter and the Java class that implements the filter. The <filter-mapping> element defines the URL pattern that specifies which targets the <filter-name> element should apply to. In this simple example, the filter applies to only one target: the JSP code in filter.jsp.

Invoke filter.jsp from your Web browser. The console output should look like this:

![image](https://github.com/user-attachments/assets/99450d7a-0d1c-412f-81e9-00e30624292e)

Reference: [Servlet Filters and Event Listeners (oracle.com)](https://docs.oracle.com/cd/B14099_19/web.1012/b14017/filters.htm)


