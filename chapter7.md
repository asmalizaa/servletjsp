# Custom Tag

_Updated on 22nd February 2024
Use this example instead Custom Tags in JSP - GeeksforGeeks
https://www.geeksforgeeks.org/custom-tags-in-jsp/_

A custom tag is a user defined JSP language element. When a JSP page containing a custom tag is translated into a servlet, the tag is converted to operations on an object called a tag handler. The Web container then invokes those operations when the JSP page's servlet is executed.

JSP tag extensions lets you create new tags that you can insert directly into a JavaServer Page. The JSP 2.0 specification introduced the Simple Tag Handlers for writing these custom tags.

To write a custom tag, you can simply extend SimpleTagSupport class and override the doTag() method, where you can place your code to generate content for the tag.

## Create "Hello" Tag

Consider you want to define a custom tag named <ex:Hello> and you want to use it in the following fashion without a body.

```
<ex:Hello/>
```

To create a custom JSP tag, you must first create a Java class that acts as a tag handler. Let us now create the HelloTag class as follows.

```
package com.example.tags;

import java.io.IOException;

import jakarta.servlet.jsp.JspException;
import jakarta.servlet.jsp.JspWriter;
import jakarta.servlet.jsp.tagext.SimpleTagSupport;

public class HelloTag extends SimpleTagSupport {

	@Override
	public void doTag() throws JspException, IOException {
		// get writer object
		JspWriter out = getJspContext().getOut();

		// display message
		out.println("Hello Custom Tag!");
	}
}
```

The above code has simple coding where the doTag() method takes the current JspContext object using the getJspContext() method and uses it to send "Hello Custom Tag!" to the current JspWriter object.

Let us compile the above class and copy it in a directory available in the environment variable CLASSPATH. Finally, create the following tag library file: <Tomcat-Installation-Directory>webapps\ROOT\WEB-INF\custom.tld.

```
<?xml version="1.0" encoding="UTF-8"?>
<taglib>
	<tlib-version>1.0</tlib-version>
	<jsp-version>2.0</jsp-version>
	<short-name>Example Custom Tag</short-name>

	<tag>
		<name>Hello</name>
		<tag-class>com.example.tags.HelloTag</tag-class>
		<body-content>empty</body-content>
	</tag>
	
</taglib>
```

Let us now use the above defined custom tag Hello in our JSP program as follows.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%@ taglib uri="WEB-INF/custom.tld" prefix="ex" %>
<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<c:set var="pageTitle" value="Custom Tag Example" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<h1><c:out value="${pageTitle}" /></h1>
	<ex:Hello/>
	
	<br/><br/>
	<a href="LoginSuccess.jsp">Go Back</a>
	
</body>
</html>
```

Call the above JSP and this should produce the following result.

![image](https://github.com/user-attachments/assets/38e950c6-5796-48c2-9f08-ec8de0926407)

## Accessing the Tag Body

You can include a message in the body of the tag as you have seen with standard tags. Consider you want to define a custom tag named <ex:Hello> and you want to use it in the following fashion with a body.

![image](https://github.com/user-attachments/assets/0d7b75b7-9d18-41f5-b970-d1778630557e)

Let us make the following changes in the above tag code to process the body of the tag.

![image](https://github.com/user-attachments/assets/6cfddd5e-7012-4389-ad59-b4c8dfaf3837)

Here, the output resulting from the invocation is first captured into a StringWriter before being written to the JspWriter associated with the tag. We need to change TLD file as follows.

![image](https://github.com/user-attachments/assets/fdd39962-86bf-4f94-9abf-3ef6197f29e4)

Let us now call the above tag with proper body as follows.

![image](https://github.com/user-attachments/assets/80c72939-9bfc-4bab-b568-7114fce80127)

You will receive the following result.

![image](https://github.com/user-attachments/assets/91efb377-61c0-4d2d-8368-c86ffb5d5358)

## Custom Tag Attributes

You can use various attributes along with your custom tags. To accept an attribute value, a custom tag class needs to implement the setter methods, identical to the JavaBean setter methods as shown below.

![image](https://github.com/user-attachments/assets/b2b7e4c8-b8b2-4473-9030-1839acac061b)

The attribute's name is "message", so the setter method is setMessage(). Let us now add this attribute in the TLD file using the <attribute> element as follows.

![image](https://github.com/user-attachments/assets/6184c3a6-8bab-441d-9657-f5ffbd8e155e)

Let us follow JSP with message attribute as follows.

![image](https://github.com/user-attachments/assets/c7ea4428-9465-40d2-a7ef-85b0aae0ab9a)

This will produce following result.

![image](https://github.com/user-attachments/assets/472c5b06-4b07-4c77-bd26-7d0dac55659b)

## Custom Tag Attributes Considerations

Consider including the following properties for an attribute.

1. **name**: The name element defines the name of an attribute. Each attribute name must be unique for a particular tag.
2. **required**: This specifies if this attribute is required or is an optional one. It would be false for optional.
3. **rtexprvalue**: Declares if a runtime expression value for a tag attribute is valid
4. **type**: Defines the Java class-type of this attribute. By default it is assumed as String
5. **description**: Informational description can be provided.
6. **fragment**: Declares if this attribute value should be treated as a JspFragment.

Following is the example to specify properties related to an attribute.

![image](https://github.com/user-attachments/assets/c4e68109-ec03-4d27-b23f-5870b1a6d35e)

If you are using two attributes, then you can modify your TLD as follows.

![image](https://github.com/user-attachments/assets/233802e4-3178-41e8-998f-329df5b0d42a)
