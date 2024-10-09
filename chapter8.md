# Tag Files

A tag file is a source file that contains a fragment of JSP code that is reusable as a custom tag. Tag files allow you to create custom tags using JSP syntax. Just as a JSP page gets translated into a servlet class and then compiled, a tag file gets translated into a tag handler and then compiled.

The recommended file extension for a tag file is .tag. As is the case with JSP files, the tag can be composed of a top file that includes other files that contain either a complete tag or a fragment of a tag file. Just as the recommended extension for a fragment of a JSP file is .jspf, the recommended extension for a fragment of a tag file is .tagf.

Much like custom tags and EL functions, tag files promote separation of concerns by keeping the business logic apart from the presentation. So why do we need yet another method of creating a custom action? There are several reasons why we may want to use tag files for developing new software, instead of custom tags or EL functions, as outlined below:

- With tag files there is no need to precompile tag handlers as you have to with custom tags, or a Java class when using an EL function; tag files get compiled the first time they are invoked.
- There is no need for a Tag Library Descriptor (TLD) when using tag files as the name of the tag file is the same as the custom action it represents.
- Tag files can be written completely in JSP syntax meaning page designers with no Java experience can create them.

The container and any JSP pages that want to make use of tag files need a way to locate the tag file in question and this is achieved by deploying the tag file into certain places within our deployment file structure. The container will search in several places as outlined below so make sure that any tag files you create are in one of the locations listed below:

- Directly inside the WEB-INF/tags folder
- Directly inside a sub-directory of WEB-INF/tags folder
- Inside the META-INF/tags directory inside a JAR file that's inside the WEB-INF/lib folder
- Inside a sub-directory of the META-INF/tags directory, inside a JAR file that's inside the WEB-INF/lib folder

## Creating a Tag File

Tag files use the tag directive which is very similar to the page directive used by JSP pages, more on directives a bit later in the lesson. For now we have enough information to create our first tag file which is called tagfileone.tag and which has been placed directly inside the WEB-INF/tags folder of our web application called tagfiles.

```
<%@ tag
	import="java.time.LocalDateTime,java.time.format.DateTimeFormatter"%>

<%
	// get current date and time, then format it before display
	DateTimeFormatter formatter = DateTimeFormatter.ofPattern("dd-MM-yyy HH:mm:ss");
	String now = LocalDateTime.now().format(formatter);
	out.println(now);
%>
```

## Using a Tag File

When we want to use a tag file within our JSP pages we use the taglib directive which can reference either a relative path of the context root or an absolute path that points to the location of the tag file within one of the directories where the container looks for tag files as mentioned above.

The following code example shows a JSP page called tagfileonetest.jsp that uses the tag file tagfileone.tag we wrote above. Remember that no TLD is required as when using tag files the name of the tag file is the same as the custom action it represents, which in this case is tagfileone.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<!-- add reference to tag files -->
<%@ taglib tagdir="/WEB-INF/tags" prefix="tf" %>

<c:set var="pageTitle" value="Tag Files Example" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<h1><c:out value="${pageTitle}" /></h1>
	
	<!-- use the tagfileone tag -->
	<p>Current date and time is <tf:tagfileone /></p>
	
	<br/>
	<a href="LoginSuccess.jsp">Go Back</a>
	
</body>
</html>
```

This screenshot shows file structure within Tomcat folder c:\apache-tomcat-6.0.37\webapps\tagfiles for the above entities.

![image](https://github.com/user-attachments/assets/645b17d5-3d00-4865-ba15-29f98e6215d5)

The following screenshot shows the results of running the tagfileonetest.jsp JSP page within the tagfiles folder within Tomcat.

![image](https://github.com/user-attachments/assets/280a7b0d-9a25-4bf2-9cc3-d2c0eb1d09c1)

## Tag File Implicit Objects

With tag files we have access to implicit objects which are similar to those discussed in the JSP Implicit Objects lesson. The table below shows all the implicit objects available to tag file authors and links to the lessons in which the various types involved are discussed.

| Implicit Object | Type |
|-----------------|------|
| application | javax.servlet.ServletContext |
| config | javax.servlet.ServletConfig |
| out | javax.servlet.jsp.JspWriter |
| pageContext | javax.servlet.jsp.PageContext |
| request | javax.servlet.http.HttpServletRequest |
| response | javax.servlet.http.HttpServletResponse |
| session | javax.servlet.http.HttpSession |

To demonstrate, create another tag file with codes below. Save it in a file called tagfiletwo.tag

```
<%
	// retrieve session object
	String user = (String) session.getAttribute("user");
	out.println("Hello " + user + "!");
%>
```

![image](https://github.com/user-attachments/assets/13f842ff-2881-4298-a99e-f3edd7c56884)

Update the JSP to contain the second tag file.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<!-- add reference to tag files -->
<%@ taglib tagdir="/WEB-INF/tags" prefix="tf" %>

<c:set var="pageTitle" value="Tag Files Example" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<h1><c:out value="${pageTitle}" /></h1>
	
	<!-- use the tagfileone tag -->
	<p>Current date and time is <tf:tagfileone /></p>
	
	<!-- use the tagfiletwo tag -->
	<p><tf:tagfiletwo /></p>
	
	<br/>
	<a href="LoginSuccess.jsp">Go Back</a>
	
</body>
</html>
```

Relaunch the JSP page and verify the output similar like below.

![image](https://github.com/user-attachments/assets/bf038f3d-54f8-4c66-8040-8d8f862840e8)


Reference: [JSTL Tutorials - Tag Files (server2client.com)](https://server2client.com/jstl/tagfiles.html)

