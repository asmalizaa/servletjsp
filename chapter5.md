# Expression Language

JSP Expression Language (EL) makes it possible to easily access application data stored in JavaBeans components. JSP EL allows you to create expressions both (a) arithmetic and (b) logical. Within a JSP EL expression, you can use integers, floating point numbers, strings, the built-in constants true and false for boolean values, and null.

## Simple Syntax

Typically, when you specify an attribute value in a JSP tag, you simply use a string. For example

![image](https://github.com/user-attachments/assets/ac0b593f-ef16-470f-86f4-4e62d985e611)

JSP EL allows you to specify an expression for any of these attribute values. A simple syntax for JSP EL is as follows

![image](https://github.com/user-attachments/assets/bad33bf1-41c2-4313-b205-481111e183b8)

Here expr specifies the expression itself. The most common operators in JSP EL are . and []. These two operators allow you to access various attributes of Java Beans and built-in JSP objects.

For example, the above syntax <jsp:setProperty> tag can be written with an expression like

![image](https://github.com/user-attachments/assets/7cde5bae-e688-496a-8515-1855d213eea8)

When the JSP compiler sees the ${} form in an attribute, it generates code to evaluate the expression and substitutes the value of expression.

To deactivate the evaluation of EL expressions, we specify the isELIgnored attribute of the page directive as below

![image](https://github.com/user-attachments/assets/a99067bb-849f-4ef6-8ef6-b8c9ba7e161d)

## Using Expressions

EL expressions can be used

- In static text
- In any standard or custom tag attribute that can accept an expresion.

The value of an expression in static text is computed and inserted into the current output. If the static text appears in a tag body, note that an expression will not be evaluated if the body is declared to be tagdependent.

