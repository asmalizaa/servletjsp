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

![image](https://github.com/user-attachments/assets/918390fc-2b73-4fb9-a285-61a74f5a612a)

## Accessing the Tag Body

You can include a message in the body of the tag as you have seen with standard tags. Consider you want to define a custom tag named <ex:Hello> and you want to use it in the following fashion with a body.

```
	<ex:HelloMessage>
		This is another message from body
	</ex:HelloMessage>
```

Let us make the following changes in the above tag code to process the body of the tag.

```
package com.example.tags;

import java.io.IOException;
import java.io.StringWriter;

import jakarta.servlet.jsp.JspException;
import jakarta.servlet.jsp.tagext.SimpleTagSupport;

public class HelloMessageTag extends SimpleTagSupport {

	StringWriter sw = new StringWriter();

	@Override
	public void doTag() throws JspException, IOException {
		// get the custom message from html body
		getJspBody().invoke(sw);

		// display custom message on the page
		getJspContext().getOut().println(sw.toString());
	}
}
```

Here, the output resulting from the invocation is first captured into a StringWriter before being written to the JspWriter associated with the tag. We need to change TLD file as follows.

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

	<tag>
		<name>HelloMessage</name>
		<tag-class>com.example.tags.HelloMessageTag</tag-class>
		<body-content>scriptless</body-content>
	</tag>

</taglib>
```

Let us now call the above tag with proper body as follows. Update the JSP page to look like below.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%@ taglib uri="WEB-INF/custom.tld" prefix="ex"%>
<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<c:set var="pageTitle" value="Custom Tag Example" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<h1>
		<c:out value="${pageTitle}" />
	</h1>
	<ex:Hello />

	<br /><br />
	<ex:HelloMessage>
		This is another message from body
	</ex:HelloMessage>
	
	<br /><br />
	<a href="LoginSuccess.jsp">Go Back</a>
	
</body>
</html>
```

You will receive the following result.

![image](https://github.com/user-attachments/assets/f2f6a51f-8876-467f-9c13-2ab8afa7b200)

## Custom Tag Attributes

You can use various attributes along with your custom tags. To accept an attribute value, a custom tag class needs to implement the setter methods, identical to the JavaBean setter methods as shown below.

```
package com.example.tags;

import java.io.IOException;
import java.io.StringWriter;

import jakarta.servlet.jsp.JspException;
import jakarta.servlet.jsp.JspWriter;
import jakarta.servlet.jsp.tagext.SimpleTagSupport;

public class HelloMessageTag extends SimpleTagSupport {

	StringWriter sw = new StringWriter();

	// since we now are going to use attribute called 'message'
	private String message;

	// define set method to assign value to message
	public void setMessage(String msg) {
		message = msg;
	}

	@Override
	public void doTag() throws JspException, IOException {
		// check if message is not empty
		if (message != null) {
			// use message from attribute
			JspWriter out = getJspContext().getOut();
			out.println(message);
		} else {
			// get the custom message from html body
			getJspBody().invoke(sw);

			// display custom message on the page
			getJspContext().getOut().println(sw.toString());
		}
	}
}
```

The attribute's name is "message", so the setter method is setMessage(). Let us now add this attribute in the TLD file using the <attribute> element as follows.

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
	
		<tag>
		<name>HelloMessage</name>
		<tag-class>com.example.tags.HelloMessageTag</tag-class>
		<body-content>scriptless</body-content>

		<attribute>
			<name>message</name>
		</attribute>
	</tag>
	
</taglib>
```

Let us follow JSP with message attribute as follows.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%@ taglib uri="WEB-INF/custom.tld" prefix="ex"%>
<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<c:set var="pageTitle" value="Custom Tag Example" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<h1>
		<c:out value="${pageTitle}" />
	</h1>
	<ex:Hello />

	<br /><br />
	<ex:HelloMessage>
		This is another message from body
	</ex:HelloMessage>
	
	<br/><br/>
	<ex:HelloMessage message="This is the message body that's assigned using attribute" />
	
	<br /><br />
	<a href="LoginSuccess.jsp">Go Back</a>
	
</body>
</html>
```

This will produce following result.

![image](https://github.com/user-attachments/assets/1d067487-1db2-4c75-9b87-fb5c9fbb431a)

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
