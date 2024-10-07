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

There are three ways to set a tag attribute value:

- With a single expression construct:

  ```
  <some:tag value="${expr}"/>
  ```

  The expression is evaluated, and the result is coerced to the attribute's expected type.
  
- With one or more expressions separated or surrounded by text:

  ```
  <some:tag value="some${expr}${expr}text${expr}"/>
  ```

  The expressions are evaluated from left to right. Each expression is coerced to a String and then concatenated with any intervening text. The resulting String is then coerced to the attribute's expected type.

- With text only:

  ```
  <some:tag value="sometext"/>
  ```

  In this case, the attribute's String value is coerced to the attribute's expected type.

Expressions used to set attribute values are evaluated in the context of an expected type. If the result of the expression evaluation does not match the expected type exactly, a type conversion will be performed. For example, the expression ${1.2E4 + 1.4} provided as the value of an attribute of type float will result in the following conversion:

![image](https://github.com/user-attachments/assets/b96920a6-193d-4bf2-b6fe-c69a601057de)

## Variables

The Web container evaluates a variable that appears in an expression by looking up its value according to the behavior of PageContext.findAttribute(String). 

For example, when evaluating the expression ${product}, the container will look for product in the page, request, session, and application scopes and will return its value. If product is not found, null is returned. 

A variable that matches one of the implicit objects described in Implicit Objects will return that implicit object instead of the variable's value.

Properties of variables are accessed using the . operator and can be nested arbitrarily.

The JSP expression language unifies the treatment of the . and [] operators. expr-a.expr-b is equivalent to a["expr-b"]; that is, the expression expr-b is used to construct a literal whose value is the identifier, and then the [] operator is used with that value.

To evaluate expr-a[expr-b], evaluate expr-a into value-a and evaluate expr-b into value-b. If either value-a or value-b is null, return null.

- If value-a is a Map, return value-a.get(value-b). If !value-a.containsKey(value-b), then return null.
- If value-a is a List or array, coerce value-b to int and return value-a.get(value-b) or Array.get(value-a, value-b), as appropriate. If the coercion couldn't be performed, an error is returned. If the get call returns an IndexOutOfBoundsException, null is returned. If the get call returns another exception, an error is returned.
- If value-a is a JavaBeans object, coerce value-b to String. If value-b is a readable property of value-a, then return the result of a get call. If the get method throws an exception, an error is returned.

## Implicit Objects

The JSP expression language defines a set of implicit objects:

- pageContext: The context for the JSP page. Provides access to various objects including:
  - servletContext: The context for the JSP page's servlet and any Web components contained in the same application.
  - session: The session object for the client.
  - request: The request triggering the execution of the JSP page.
  - response: The response returned by the JSP page.

In addition, several implicit objects are available that allow easy access to the following objects:

- param: Maps a request parameter name to a single value
- paramValues: Maps a request parameter name to an array of values
- header: Maps a request header name to a single value
- headerValues: Maps a request header name to an array of values
- cookie: Maps a cookie name to a single cookie
- initParam: Maps a context initialization parameter name to a single value

Finally, there are objects that allow access to the various scoped variables described in Using Scope Objects.

- pageScope: Maps page-scoped variable names to their values
- requestScope: Maps request-scoped variable names to their values
- sessionScope: Maps session-scoped variable names to their value
- applicationScope: Maps application-scoped variable names to their values

When an expression reference one of these objects by name, the appropriate object is returned instead of the corresponding attribute. For example, ${pageContext} returns the PageContext object, even if there is an existing pageContext attribute containing some other value.

## Literals

The JSP expression language defines the following literals:

- Boolean: true and false
- Integer: as in Java
- Floating point: as in Java
- String: with single and double quotes; " is escaped as \", ' is escaped as \', and \ is escaped as \\.
- Null: null

## Operators

In addition to the . and [] operators discussed in Variables, the JSP expression language provides the following operators:

- Arithmetic: +, - (binary), *, / and div, % and mod, - (unary)
- Logical: and, &&, or, ||, not, !
- Relational: ==, eq, !=, ne, <, lt, >, gt, <=, ge, >=, le. Comparisons can be made against other values, or against boolean, string, integer, or floating point literals.
- Empty: The empty operator is a prefix operation that can be used to determine whether a value is null or empty
- Conditional: A ? B : C. Evaluate B or C, depending on the result of the evaluation of A.

The precedence of operators highest to lowest, left to right is as follows:

- [] .
- () - Used to change the precedence of operators.
  - (unary) not ! empty
- / div % mod
- + - (binary)
- < > <= >= lt gt le ge
- == != eq ne
- && and
- || or
- ? :

## Reserved Words

The following words are reserved for the JSP expression language and should not be used as identifiers.

<table>
  <tr><td>and</td><td>eq</td><td>gt</td><td>true</td><td>instanceof</td></tr>
  <tr><td>or</td><td>ne</td><td>le</td><td>false</td><td>empty</td></tr>
  <tr><td>not</td><td>lt</td><td>ge</td><td>null</td><td>div</td></tr>
  <tr><td>mod</td><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td><td>&nbsp;</td></tr>
</table>

Note that many of these words are not in the language now, but they may be in the future, so you should avoid using them.

## EL param example

In this example, we printing the data stored in the session scope using EL. For this purpose, we have used sessionScope object.

index.jsp

![image](https://github.com/user-attachments/assets/bc88c1dd-d1e1-45b4-ae29-13ae6ece5a42)

process.jsp

![image](https://github.com/user-attachments/assets/8663a466-0ac6-4b06-8ad4-e9a677c05f48)

## EL session example

In this example, we printing the data stored in the session scope using EL. For this purpose, we have used sessionScope object.

index.jsp

![image](https://github.com/user-attachments/assets/f151da96-2a79-485d-b170-7cc3f71d30cc)

process.jsp

![image](https://github.com/user-attachments/assets/16055f09-80b1-4a17-94b8-10ee1138cf07)

## EL cookie example

index.jsp

![image](https://github.com/user-attachments/assets/e5b9f0b0-4f43-4c25-b584-d313a9754847)

process.jsp

![image](https://github.com/user-attachments/assets/46b2f748-3f79-425f-a362-8646e11f0bbb)



