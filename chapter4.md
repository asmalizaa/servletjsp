# Session Management

## What is a Session?

HTTP protocol and Web Servers are stateless, what it means is that for web server every request is a new request to process, and they can’t identify if it’s coming from client that has been sending request previously. 

But sometimes in web applications, we should know who the client is and process the request accordingly. 

For example, a shopping cart application should know who is sending the request to add an item and in which cart the item has to be added or who is sending checkout request so that it can charge the amount to correct client. 

Session is a conversional state between client and server, and it can consist of multiple request and response between client and server. 

Since HTTP and Web Server both are stateless, the only way to maintain a session is when some unique information about the session (session id) is passed between server and client in every request and response. 

There are several ways through which we can provide unique identifier in request and response.

1. **User Authentication** - This is the very common way where we user can provide authentication credentials from the login page and then we can pass the authentication information between server and client to maintain the session. This is not very effective method because it won’t work if the same user is logged in from different browsers.
2. **HTML Hidden Field** - We can create a unique hidden field in the HTML and when user starts navigating, we can set its value unique to the user and keep track of the session. This method can’t be used with links because it needs the form to be submitted every time request is made from client to server with the hidden field. Also, it’s not secure because we can get the hidden field value from the HTML source and use it to hack the session.
3. **URL Rewriting** - We can append a session identifier parameter with every request and response to keep track of the session. This is very tedious because we need to keep track of this parameter in every response and make sure it’s not clashing with other parameters.
4. **Cookies** - Cookies are small piece of information that is sent by web server in response header and gets stored in the browser cookies. When client make further request, it adds the cookie to the request header, and we can utilize it to keep track of the session. We can maintain a session with cookies but if the client disables the cookies, then it won’t work.
5. **Session Management API** - Session Management API is built on top of above methods for session tracking. Some of the major disadvantages of all the above methods are:

   a.	Most of the time we don’t want to only track the session, we have to store some data into the session that we can use in future requests. This will require a lot of effort if we try to implement this.
   b.	All the above methods are not complete in themselves, all of them won’t work in a particular scenario. So, we need a solution that can utilize these methods of session tracking to provide session management in all cases.

## Cookies

Cookies are used a lot in web applications to personalize response based on your choice or to keep track of session. Before moving forward to the Servlet Session Management API, Let’s see how we can keep track of session with cookies through a small web application. 

# Activity

Let's add a login page to our project. This login page will then be configured as welcome page where we will get authentication details from user.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Login</title>
</head>
<body>
	<h1>Login</h1>
	<form action="LoginServlet" method="post">
		Username: <input type="text" name="username"><br/>
		Password: <input type="password" name="userpassword"/><br/>
		<input type="submit" value=""/>
	</form>
</body>
</html>
```

To process the login, we are going to add a servlet called LoginServlet.

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class LoginServlet extends HttpServlet {

	// to keep it simple, we are going to use this default credentials
	private final String username = "user";
	private final String password = "password";

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// get request parameters for username and userpassword
		String user = req.getParameter("username");
		String pwd = req.getParameter("userpassword");

		// check credentials
		if (user.equals(username) && pwd.equals(password)) {
			// valid - add to cookie
			Cookie loginCookie = new Cookie("user", user);

			// set cookie to expiry in 30 minutes
			loginCookie.setMaxAge(30 * 60);

			// add cookie to response
			resp.addCookie(loginCookie);

			// redirect to index page
			resp.sendRedirect("LoginSuccess.jsp");

		} else {
			// invalid - replay login page
			RequestDispatcher rd = getServletContext().getRequestDispatcher("/login.html");
			rd.include(req, resp);
		}
	}
}
```

Notice the cookie that we are setting to the response and then forwarding it to LoginSucceess.jsp, at this moment it html cannot handle cookie. So, we are going to convert it to a JSP so this cookie can be used there to track the session. 

Also notice that cookie timeout is set to 30 minutes. Ideally there should be a complex logic to set the cookie value for session tracking so that it won’t collide with any other request.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Login Success Page</title>
</head>
<body>
	<h1>Login Success Page</h1>

	<!-- add java codes to check cookie -->
	<%
	String user = null;

	// retrieve cookies
	Cookie[] cookies = request.getCookies();

	if (cookies != null) {
		for (Cookie cookie : cookies) {
			if (cookie.getName().equals("user")) {
		user = cookie.getValue();
			}
		}
	}

	// if user is empty, redirect to login page
	if (user == null) {
		response.sendRedirect("login.html");
	}
	%>
	
	<!-- else display greeting -->
	<h3>Hi <%= user %>, Good day!</h3>
	
	<!-- add logout button -->
	<form action="LogoutServlet" method="post">
		<input type="submit" value="Logout" />
	</form>
</body>
</html>
```

Notice that if we try to access the JSP directly, it will forward us to the login page. 

When we will click on Logout button, we should make sure that cookie is removed from client browser.

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

public class LogoutServlet extends HttpServlet {

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// retrieve cookies
		Cookie[] cookies = request.getCookies();
		Cookie loginCookie = null;

		if (cookies != null) {
			for (Cookie cookie : cookies) {
				if (cookie.getName().equals("user")) {
					loginCookie = cookie;
					break;
				}
			}
		}

		if (loginCookie != null) {
			// set cookie's max age to 0
			loginCookie.setMaxAge(0);
			response.addCookie(loginCookie);
		}

		response.sendRedirect("login.html");
	}

}
```

There is no method to remove the cookie, but we can set the maximum age to 0 so that it will be deleted from client browser immediately. When we run above application, we get response like below images.

![image](https://github.com/user-attachments/assets/919c53aa-d150-4109-abad-aefd54c3481f)

![image](https://github.com/user-attachments/assets/e2b473dc-32c0-42e8-8a87-1cc6681bd965)

After successfully logged in, click on Logout button. If you then try to access the LoginSuccess.jsp directly, it will redirect you to the login.html page.

## HttpSession

Servlet API provides Session management through HttpSession interface. We can get session from HttpServletRequest object using following methods. HttpSession allows us to set objects as attributes that can be retrieved in future requests.

- **HttpSession getSession()** - This method always returns a HttpSession object. It returns the session object attached with the request, if the request has no session attached, then it creates a new session and return it.
- **HttpSession getSession(boolean flag)** - This method returns HttpSession object if request has session else it returns null.

Some of the important methods of HttpSession are:

1. **String getId()** - Returns a string containing the unique identifier assigned to this session.
2. **Object getAttribute(String name)** - Returns the object bound with the specified name in this session, or null if no object is bound under the name. Some other methods to work with Session attributes are getAttributeNames(), removeAttribute(String name) and setAttribute(String name, Object value).
3. **long getCreationTime()** - Returns the time when this session was created, measured in milliseconds since midnight January 1, 1970 GMT. We can get last accessed time with getLastAccessedTime() method.
4. **setMaxInactiveInterval(int interval)** - Specifies the time, in seconds, between client requests before the servlet container will invalidate this session. We can get session timeout value from getMaxInactiveInterval() method.
5. **ServletContext getServletContext()** - Returns ServletContext object for the application.
6. **boolean isNew()** - Returns true if the client does not yet know about the session or if the client chooses not to join the session.
7. **void invalidate()** - Invalidates this session then unbinds any objects bound to it.

### Understanding JSESSIONID Cookie

When we use HttpServletRequest getSession() method and it creates a new request, it creates the new HttpSession object and also add a Cookie to the response object with name JSESSIONID and value as session id. 

This cookie is used to identify the HttpSession object in further requests from client. 

If the cookies are disabled at client side and we are using URL rewriting then this method uses the jsessionid value from the request URL to find the corresponding session. 

JSESSIONID cookie is used for session tracking, so we should not use it for our application purposes to avoid any session related issues. 

# Activity

Let’s see example of session management using HttpSession object. We will repeat the login process again but this time, we are going to use session instead.

First, update the LoginServlet to include codes that will use session.

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LoginServlet extends HttpServlet {

	// to keep it simple, we are going to use this default credentials
	private final String username = "user";
	private final String password = "password";

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// get request parameters for username and userpassword
		String user = req.getParameter("username");
		String pwd = req.getParameter("userpassword");

		// check credentials
		if (user.equals(username) && pwd.equals(password)) {
			// upon successful login, get session from request object
			HttpSession session = req.getSession();
			
			// set session to expire in 30 minutes
			session.setMaxInactiveInterval(30 * 60);
			
			// add user to session
			session.setAttribute("user", user);
			
			// valid - add to cookie
			Cookie loginCookie = new Cookie("user", user);

			// set cookie to expiry in 30 minutes
			loginCookie.setMaxAge(30 * 60);

			// add cookie to response
			resp.addCookie(loginCookie);

			// redirect to index page
			resp.sendRedirect("LoginSuccess.jsp");

		} else {
			// invalid - replay login page
			RequestDispatcher rd = getServletContext().getRequestDispatcher("/login.html");
			rd.include(req, resp);
		}
	}
}
```

Next, update LoginSuccess.jsp to access the session.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Login Success Page</title>
</head>
<body>
	<h1>Login Success Page</h1>

	<!-- add java codes to check cookie -->
	<%
	String user = null;
	String jsessionid = null;

	// allow access only is session existed
	if (session.getAttribute("user") != null) {
		user = (String) session.getAttribute("user");
	}

	// retrieve cookies
	Cookie[] cookies = request.getCookies();

	if (cookies != null) {
		for (Cookie cookie : cookies) {
			if (cookie.getName().equals("user")) {
		user = cookie.getValue();
			}

			// retrieve jsessionid cookie
			if (cookie.getName().equals("JSESSIONID")) {
		jsessionid = cookie.getValue();
			}
		}
	}

	// if user is empty, redirect to login page
	if (user == null) {
		response.sendRedirect("login.html");
	}
	%>

	<!-- else display greeting -->
	<h3>
		<p>
			Hi
			<%=user%>, Good day!
		</p>

		<%
		if (jsessionid != null)
			out.println("Your jsession id is " + jsessionid);
		%>

	</h3>

	<!-- add logout button -->
	<form action="LogoutServlet" method="post">
		<input type="submit" value="Logout" />
	</form>
</body>
</html>
```

Finally, in the LogoutServlet we need to invalid session upon logout.

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LogoutServlet extends HttpServlet {

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// retrieve cookies
		Cookie[] cookies = request.getCookies();
		Cookie loginCookie = null;

		if (cookies != null) {
			for (Cookie cookie : cookies) {
				if (cookie.getName().equals("user")) {
					loginCookie = cookie;
					break;
				}
			}
		}

		if (loginCookie != null) {
			// set cookie's max age to 0
			loginCookie.setMaxAge(0);
			response.addCookie(loginCookie);
		}

		// invalidate session
		HttpSession session = request.getSession();
		System.out.println("Logging out user: " + session.getAttribute("user"));
		if (session != null) {
			session.invalidate();
		}

		response.sendRedirect("login.html");
	}

}
```

## URL Rewriting

As we saw in last section that we can manage a session with HttpSession but if we disable the cookies in browser, it won’t work because server will not receive the JSESSIONID cookie from client. 

Servlet API provides support for URL rewriting that we can use to manage session in this case. 

The best part is that from coding point of view, it’s very easy to use and involves one step - encoding the URL. 

Another good thing with Servlet URL Encoding is that it’s a fallback approach and it kicks in only if browser cookies are disabled. 

We can encode URL with HttpServletResponse encodeURL() method and if we have to redirect the request to another resource and we want to provide session information, we can use encodeRedirectURL() method. 

# Activity

Update the LoginServlet file to perform the URL encoding.

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LoginServlet extends HttpServlet {

	// to keep it simple, we are going to use this default credentials
	private final String username = "user";
	private final String password = "password";

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		// get request parameters for username and userpassword
		String user = req.getParameter("username");
		String pwd = req.getParameter("userpassword");

		// check credentials
		if (user.equals(username) && pwd.equals(password)) {
			// upon successful login, get session from request object
			HttpSession session = req.getSession();

			// set session to expire in 30 minutes
			session.setMaxInactiveInterval(30 * 60);

			// add user to session
			session.setAttribute("user", user);

			// valid - add to cookie
			Cookie loginCookie = new Cookie("user", user);

			// set cookie to expiry in 30 minutes
			loginCookie.setMaxAge(30 * 60);

			// add cookie to response
			resp.addCookie(loginCookie);

//			// redirect to index page
//			resp.sendRedirect("LoginSuccess.jsp");

			// get the encoded url string
			String encodedURL = resp.encodeRedirectURL("LoginSuccess.jsp");
			resp.sendRedirect(encodedURL);

		} else {
			// invalid - replay login page
			RequestDispatcher rd = getServletContext().getRequestDispatcher("/login.html");
			rd.include(req, resp);
		}
	}
}
```

Next is to encode every URL in your application as a fallback whenever cookie is disabled in the client.

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Login Success Page</title>
</head>
<body>
	<h1>Login Success Page</h1>

	<!-- add java codes to check cookie -->
	<%
	String user = null;
	String jsessionid = null;

	// allow access only is session existed
	if (session.getAttribute("user") != null) {
		user = (String) session.getAttribute("user");
	}

	// retrieve cookies
	Cookie[] cookies = request.getCookies();

	if (cookies != null) {
		for (Cookie cookie : cookies) {
			if (cookie.getName().equals("user")) {
		user = cookie.getValue();
			}

			// retrieve jsessionid cookie
			if (cookie.getName().equals("JSESSIONID")) {
		jsessionid = cookie.getValue();
			}
		}
	}

	// if user is empty, redirect to login page
	if (user == null) {
		response.sendRedirect("login.html");
	}
	%>

	<!-- else display greeting -->
	<h3>
		<p>
			Hi
			<%=user%>, Good day!
		</p>

		<%
		if (jsessionid != null)
			out.println("Your jsession id is " + jsessionid);
		%>

	</h3>

	<!-- add logout button -->
	<form action="<% response.encodeURL("LogoutServlet"); %>" method="post">
		<input type="submit" value="Logout" />
	</form>
</body>
</html>
```

```
package com.example.servlet;

import java.io.IOException;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.Cookie;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LogoutServlet extends HttpServlet {

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// retrieve cookies
		Cookie[] cookies = request.getCookies();
		Cookie loginCookie = null;

		if (cookies != null) {
			for (Cookie cookie : cookies) {
				if (cookie.getName().equals("user")) {
					loginCookie = cookie;
					break;
				}
			}
		}

		if (loginCookie != null) {
			// set cookie's max age to 0
			loginCookie.setMaxAge(0);
			response.addCookie(loginCookie);
		}

		// invalidate session
		HttpSession session = request.getSession();
		System.out.println("Logging out user: " + session.getAttribute("user"));
		if (session != null) {
			session.invalidate();
		}

		// no encoding since we have invalidated the session
		response.sendRedirect("login.html");
	}

}
```
When we run this application with cookie disabled, the jsessionid will not be displayed on the page, but it will be appended on the address bar instead.

![image](https://github.com/user-attachments/assets/2956302b-09e4-42b4-ad51-ac147158ce71)

