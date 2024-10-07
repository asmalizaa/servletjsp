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




