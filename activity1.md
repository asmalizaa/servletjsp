# Activity 1

## Starting with first Servlet Application

To get started with Servlets, let’s first start with a simple Servlet application i.e LifeCycle application, that will demonstrate the implementation of the init(), service() and destroy() methods.

First of all, it is important to understand that if we are developing any Servlet application, it will handle some client’s request so, whenever we talk about Servlets we need to develop a index.html page (can be any other name also) which will request a particular Servlet to handle the request made by the client (in this case index.html page).

To be simple, lets first describe the steps to develop the LifeCycle application: 

- Creating the index.html page
- Creating the LifeCycle Servlet
- Creating deployment descriptor

### Creating the index.html page

For the sake of simplicity, this page will just have a button invoke life cycle. When you will click this button it will call LifeCycleServlet (which is mapped according to the entry in web.xml file).

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Registration Form</title>
</head>
<body>
	<h1>Registration Form</h1>
	<form action="LifeCycleServlet">
		<input type="submit" value="invoke life cycle servlet">
	</form>
</body>
</html>
```

The name of the Servlet is given in action attribute of form tag to which the request will be send on clicking the button, in this case FirstServlet.

### Creating the Servlet (LifeCycleServlet)

Now, its time to create the LifeCycleServlet which implements init(), service() and destroy() methods to demonstrate the lifecycle of a Servlet.

```
package com.example.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import jakarta.servlet.ServletConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.http.HttpServlet;

/**
 * Servlet implementation class LifeCycleServlet
 */
public class LifeCycleServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	ServletConfig config = null;

	// init method
	public void init(ServletConfig sc) {
		config = sc;
		System.out.println("in init");
	}

	// service method
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
		res.setContentType("text/html");
		PrintWriter pw = res.getWriter();
		pw.println("<h2>hello from life cycle servlet</h2>");
		System.out.println("in service");
	}

	// destroy method
	public void destroy() {
		System.out.println("in destroy");
	}

	public String getServletInfo() {
		return "LifeCycleServlet";
	}

	public ServletConfig getServletConfig() {
		return config; // getServletConfig
	}

}
```

### Creating deployment descriptor(web.xml) (Optional)

As discussed in other posts about web.xml file we will just proceed to the creation of it in this article.

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="https://jakarta.ee/xml/ns/jakartaee"
	xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
	id="WebApp_ID" version="6.0">
	<display-name>servletjsp</display-name>
	<welcome-file-list>
		<welcome-file>index.html</welcome-file>
		<welcome-file>index.jsp</welcome-file>
		<welcome-file>index.htm</welcome-file>
		<welcome-file>default.html</welcome-file>
		<welcome-file>default.jsp</welcome-file>
		<welcome-file>default.htm</welcome-file>
	</welcome-file-list>
	<servlet>
		<description></description>
		<display-name>LifeCycleServlet</display-name>
		<servlet-name>LifeCycleServlet</servlet-name>
		<servlet-class>com.example.servlet.LifeCycleServlet</servlet-class>
	</servlet>
	<servlet-mapping>
		<servlet-name>LifeCycleServlet</servlet-name>
		<url-pattern>/LifeCycleServlet</url-pattern>
	</servlet-mapping>
</web-app>
```

### Run the program

It is important to make sure that you have some server like Apache Tomcat installed and configured with the IDE of your choice like Eclipse.

Now, if the above condition is fulfilled then you can simply create the above three files under Web application project and then simply run the above application.

First of all the index.html file gets executed and then when the button is clicked the request goes to the Servlet, in this case LifeCycleServlet and the service() method handles the request.

![image](https://github.com/user-attachments/assets/afd94e98-1e4e-429e-bd04-3ecccfc6f97b)

When the above invoke life cycle servlet button is clicked the code under service() method of LifeCycleServlet is executed and the below output is obtained :

![image](https://github.com/user-attachments/assets/6c05d5f3-92bb-4ad9-a895-fd2e2c9d8e4e)

End of activity.
