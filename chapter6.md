# JSTL

JSTL aims to provide an easy way to maintain JSP pages. The use of tags defined in JSTL has Simplified the task of the designers to create Web pages. They can now simply use a tag related to the task that they need to implement in a JSP page. The main features of JSTL are as follows:

- Provides support for conditional processing and Uniform Resource Locator (URL)-related actions to process URL resources in a JSP page. You can also use the JSTL core tag library that provides iterator tags used to easily iterate through a collection of objects.
- Provides the XML tag library, which helps you to manipulate XML documents and perform actions related to conditional and iteration processing on parsed XML documents.
- Enables Web applications to be accessed globally by providing the internationalization tag library Internationalization means that an application can be created to adapt to various locales so that people of different regions can access the application in their native languages The internationalization tag library makes the implementation of localization in an application easy, fast and effective.
- Enables interaction with relational databases by using various SQL commands Web applications require databases to store information required for the application, which can be manipulated by using the SQL tag library provided by JSTL.
- Provides a series of functions to perform manipulations, such as checking whether an input String contains the substring specified as a parameter to a function or returning the number of items in a collection, or the number of characters in a 5tring These functions can be used in an El expression and are provided by the functions tag library.

## Tag Libraries in JSTL

A tag library provides a number of predefined actions that behind functionalities to a specific JSP page. JSTL provides tag libraries that include a wide range of actions to perform common tasks. 

For example, if you want to access data from database, you can use SQL tag library in your applications. JSTL is a standard tag library that is composed of five tag libraries. Each of these tag libraries represents separate functional area and is used with a prefix. 

Below table describes the tag libraries available in JSTL.

| Name of the Tag Library | Function | URI | Prefix |
|-------------------------|----------|-----|--------|
| core | variable support <br/>
Flow Control <br/>
Iterator <br/>
URL management <br/>
Miscellaneous <br/>
Core | http://java.sun.com/jsp/jstl/core | c |
| xml | Flow Control <br/>
Transformation <br/>
Locale | http://java.sun.com/jsp/jstl/xml | x |
| internationalization | Message Formatting <br/>
Number and date formatting | http://java.sun.com/jsp/jstl/fmt | fmt |
| sql | Database manipulation | http://java.sun.com/jsp/jstl/sql | sql |
| function | Collection length <br/>
String manipulation | http://java.sun.com/jsp/jstl/functions | fn |

## Example of JSTL

NOTE: Use this example instead [JSTL Tutorial, JSTL Tags Example | DigitalOcean](https://www.digitalocean.com/community/tutorials/jstl-tutorial-jstl-tags-example)

### The <c:out> Tag

c:out is a tag used to display the result of an expression in the web browser, which works similarly to the way JSP's <%=...%> expression tag works. The only difference is that this tag helps avoid HTML characters so that you can avoid cross-site scripting.

![image](https://github.com/user-attachments/assets/8e624ce3-b56c-4ce9-a071-ad84a837840a)

### Attributes

| Attribute | Required | Default | Description |
|-----------|----------|---------|-------------|
| value | No | Body | This attribute is to evaluate an expression and save information. |
| target | No | None | This attribute is used to display the name of an object to save information |
| property | No | None | This attribute is the property name of the object to set the value specified by the target attribute. |
| scope | No | Page | This attribute defines the variable's scope to save the information. Valid values are page, request, session, and application.  |

The following is a detail of the values available in the scope:

- page: variables set with c:set in this scope are accessible only within the current JSP page.
- request: variables set with c:set in this scope are accessible throughout the current request. This means the variables are accessible in all JSP pages involved in the same request, including any forwarded or included pages.
- session: variables set with c:set in this scope are accessible for the entire session.
- application: variables set with c:set in this scope are accessible for all application users.

Variable can be set by using the <c:set> tag within the body of another tag as in the following code snippet:

![image](https://github.com/user-attachments/assets/6879c200-2840-4917-ba60-f030c78a49c2)

In the preceding code snippet, the value for the bookname variable is set in the body of the <c:set> tag. Here <c:out> tag is used for printing the output.

Here is an example where this <c:set> Tag stores information on the session scope:

![image](https://github.com/user-attachments/assets/ed226158-1f30-4c74-aaa6-20c6c2beb87b)

The example code above sets a session scope variable using the '<c:set>' tag and later prints the value of this scope variable to the next line using the '<c:out>' tag.

![image](https://github.com/user-attachments/assets/6d06660f-e795-4dfe-83c7-db52e0716ad5)

### The <c:if> Tag

The JSP JSTL Core <c:if> tag is a conditional tag in the JSP Standard Tag Library (JSTL) core library. It includes a piece of content within the JSP page only if a specific condition is true.

![image](https://github.com/user-attachments/assets/80215617-0f4a-4f5b-8828-9f17697f1417)

### Attributes

| Attribute | Description |
|-----------|-------------|
| test | This attribute defines the condition that should be evaluated. The condition is an expression, which can be a Boolean expression or a value that can be converted to a Boolean. If the condition is true, the content within the <c:if> tag will be included in the JSP page; otherwise ignored. |

Here is an example of how the JSP JSTL Core <c:if> tag can be used within a JSP page:

![image](https://github.com/user-attachments/assets/273822ad-eac5-4e16-b44a-e53ba3b3e333)

![image](https://github.com/user-attachments/assets/8e976b82-a0fd-43c9-977d-2cceefd74ba8)

In this example, the JSTL core tag library is imported with the taglib directive at the top of the page. The <c:if> tag is then used to check if the value of the "age" request parameter is greater than or equal to 18. If the condition is true, the message "You are an adult" will be displayed on the page. If the condition is false, the message "You are not an adult" will be displayed on the page.

## How to download and install JSTL

1. Download JSTL.jar and Standard.jar file from here (or you will get these from your native Apache tomcat installation too!).
2. Now put both the files into your 'WEB-INF/lib' folder.
3. After this, add them to classpath also.
4. Finally, you can use JSTL into your project.






