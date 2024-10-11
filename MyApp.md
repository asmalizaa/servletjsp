# MyApp Sample Codes

## JSP

**login.jsp**

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link
	href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
	rel="stylesheet"
	integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
	crossorigin="anonymous">
<title>Login</title>
</head>
<body>
	<br />
	<div class="container">
		<!-- get success message from session -->
		<%
		String logout = (String) request.getAttribute("logout");
		if (logout != null) {
			out.println("<p class='text-primary'>" + logout + "</p>");
		} else {
			if (request.getAttribute("errormsg") != null) {
				out.println("<p class='text-danger'>" + request.getAttribute("errormsg") + "</p>");
			}
		}
		%>

		<div class="card text-center"">
			<div class="card-header bg-primary text-white">Login</div>
			<div class="card-body text-center">
				<form action="LoginServlet" method="post">
					<div class="form-group">
						Username: <input type="text" name="username"
							class="form-control mb-2 mr-sm-2" />
					</div>
					<div class="form-group">
						Password:<input type="password" name="userpassword"
							class="form-control mb-2 mr-sm-2" />
					</div>
					<br />
					<div class="form-group">
						<input type="submit" value="Login" class="btn btn-primary" />
					</div>
				</form>
			</div>
		</div>
	</div>
</body>
</html>
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
	integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
	crossorigin="anonymous"></script>
```

**home.jsp**

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link
	href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
	rel="stylesheet"
	integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
	crossorigin="anonymous">
<title>Home</title>
</head>
<body>
	<!-- scriplet -->
	<%
	String user = null;

	// retrieve user from session
	if (session.getAttribute("user") != null) {
		user = (String) session.getAttribute("user");
	}

	// if user is empty, redirect to login page
	if (user == null) {
		response.sendRedirect("login.jsp");
	}
	%>

	<br />
	<div class="container">
		<div class="card text-center">
			<div class="card-header bg-primary text-white">Home</div>
			<p>
				Welcome
				<%=user%>!
			</p>

			<!-- get success message from session -->
			<%
			String success = (String) session.getAttribute("success");
			if (success != null) {
				out.println("<p class='text-success'>" + success + "</p>");
			}
			%>

			<p>
				<a href="adduser.jsp">Add New User</a>
			</p>
			<p>
				<a href="UserServlet">List of Users</a>
			</p>

			<!-- add logout button -->
			<form action="LogoutServlet" method="post">
				<div class="form-group">
					<input type="submit" value="Logout" class="btn btn-primary" />
				</div>
			</form>
			<br />
		</div>
	</div>
</body>
</html>
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
	integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
	crossorigin="anonymous"></script>
```

**adduser.jsp**

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link
	href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
	rel="stylesheet"
	integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
	crossorigin="anonymous">
<title>Add New User</title>
</head>
<body>
	<br />
	<div class="container">
		<div class="card">
			<div class="card-header bg-primary text-white text-center">Add
				New User</div>
			<br />
			<div class="card-body">
				<form action="AddUser" method="post">
					<div class="form-group">
						<label for="name" class="form-label">Name</label> <input
							class="form-control" type="text" name="name"
							placeholder="yourname" />
					</div>
					<div class="form-group">
						<label for="email" class="form-label">Email</label> <input
							class="form-control" type="email" name="email"
							placeholder="yourname@example.com" />
					</div>
					<div class="form-group">
						<label for="username" class="form-label">Username</label> <input
							class="form-control" type="text" name="username"
							placeholder="username" />
					</div>
					<div class="form-group">
						<label for="password" class="form-label">Password</label> <input
							class="form-control" type="password" name="password"
							placeholder="password" />
					</div>
					<br />
					<div class="form-group">
						<input type="submit" class="btn btn-primary" value="Add User" />
					</div>
				</form>
				<br /><a href="home.jsp">Home</a><br />
			</div>
			<br />
		</div>
	</div>
</body>
</html>
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
	integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
	crossorigin="anonymous"></script>
```

**userlist.jsp**

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<%@ taglib uri="jakarta.tags.core" prefix="c"%>

<c:set var="pageTitle" value="List of Users" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<link
	href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
	rel="stylesheet"
	integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
	crossorigin="anonymous">
<title><c:out value="${pageTitle}" /></title>
</head>
<body>
	<br />
	<div class="container">
		<div class="card text-center"">
			<div class="card-header bg-primary text-white">
				<c:out value="${pageTitle}" />
			</div>
			<div class="card-body text-center">
				<!-- display list of users -->
				<table class="table table-striped">
					<thead>
						<tr>
							<th>Name</th>
							<th>Email</th>
							<th>Username</th>
							<th>Password</th>
						</tr>
					</thead>
					<tbody>
						<!-- retrieve list of users from request -->
						<!-- loop and display each user -->
						<c:forEach items="${sessionScope.users}" var="user">
							<tr>
								<td><a href="UserDetails?name=${user.name}"><c:out value="${user.name}" /></a></td>
								<td><c:out value="${user.email}" /></td>
								<td><c:out value="${user.username}" /></td>
								<td><c:out value="${user.password}" /></td>
							</tr>
						</c:forEach>
					</tbody>
				</table>
			</div>
			<a href="adduser.jsp">Add New User</a>&nbsp;<a href="home.jsp">Home</a><br/>
		</div>
	</div>
</body>
</html>
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
	integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
	crossorigin="anonymous"></script>
```

**userdetails.jsp**

```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>

<jsp:useBean id="user" class="com.example.domain.User" scope="request"></jsp:useBean>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>User Details</title>
<link
	href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css"
	rel="stylesheet"
	integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH"
	crossorigin="anonymous">
</head>
<body>
	<br />
	<div class="container">
		<div class="card text-center">
			<div class="card-header bg-primary text-white text-center">User
				Details</div>
			
			<br/>
			<p>
				Name: ${ user.name } <br /> Email: ${ user.email } <br />
				Username: ${ user.username } <br /> Password: ${ user.password }
			</p>

			<a href="userlist.jsp">Back</a>&nbsp;<a href="home.jsp">Home</a><br />
		</div>
	</div>
</body>
</html>
<script
	src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"
	integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz"
	crossorigin="anonymous"></script>
```

## Java Class and Servlets

**LoginServlet.java**

```
package com.example.login;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import com.example.domain.User;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	// to keep it simple, we are going to use this default credentials
	private final String username = "john";
	private final String password = "Pa$$w0rd";

	// create a static list of users
	private List<User> users = new ArrayList<User>();

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// get request parameters for username and userpassword
		String user = request.getParameter("username");
		String pwd = request.getParameter("userpassword");

		// get list of users
		List<User> users  = getList();
		
		// check credentials
		if (checkUser(user, pwd)) {
			// upon successful login, get session from request object
			HttpSession session = request.getSession();

			// set session to expire in 30 minutes
			session.setMaxInactiveInterval(30 * 60);

			// add user to session
			session.setAttribute("user", user);

			// add success message to session
			session.setAttribute("success", "Login successful!");

			/// get list of users and store in session
			session.setAttribute("users", users);

			// get the encoded url string
			String encodedURL = response.encodeRedirectURL("home.jsp");
			response.sendRedirect(encodedURL);

		} else {
			request.setAttribute("errormsg", "Please login with valid credentials to proceed.");
			// invalid - replay login page
			RequestDispatcher rd = getServletContext().getRequestDispatcher("/login.jsp");
			rd.include(request, response);
		}
	}

	private List<User> getList() {
		// add user to the list
		users.add(new User("John Smith", "john@example.com", "john", "123"));
		users.add(new User("Jane White", "jane@example.com", "jane", "123"));
		users.add(new User("Charles Taylor", "charles@example.com", "charles", "123"));

		return users;
	}

	private boolean checkUser(String username, String password) {
		for (User u : users) {
			if (u.getUsername().equals(username) && u.getPassword().equals(password)) {
				System.out.println("valid");
				return true;
			}
		}
		return false;
	}
}
```

**LogoutServlet.java**

```
package com.example.login;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class LogoutServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// invalidate the session
		HttpSession session = request.getSession();
		System.out.println("Logging out user: " + session.getAttribute("user"));
		if (session != null) {
			session.invalidate();
		}
		
		// add logout message to request
		request.setAttribute("logout", "You have been successfully logged out");

		// no encoding since we have invalidated the session
//		response.sendRedirect("login.jsp");
		
		RequestDispatcher rd = request.getRequestDispatcher("login.jsp");
		rd.forward(request, response);
	}
}
```

**User.java**

```
package com.example.domain;

public class User {

	private String name;
	private String email;
	private String username;
	private String password;
	
	public User() {
		// do nothing
	}
	
	public User(String name, String email, String username, String password) {
		this.name = name;
		this.email = email;
		this.username = username;
		this.password = password;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}
	
	@Override
	public String toString() {
		String header = "\n== User Details ==\n";
		return header + "Name: " + getName() + "\nEmail: " + getEmail() + "\nUsername: " + getUsername()
				+ "\nPassword: " + getPassword() + "\n";
	}
}
```

**AddUser.java**

```
package com.example.domain;

import java.io.IOException;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class AddUser extends HttpServlet {
	
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		// get session
		HttpSession session = request.getSession();
		
		// retrieve user's details
		String name = request.getParameter("name");
		String email = request.getParameter("email");
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		
		// create new user
		User newuser = new User(name, email, username, password);
		System.out.println("New user created...");
		System.out.println(newuser);
		
		// add user to session
		session.setAttribute("newuser", newuser);
		
		// redirect to user's details page
//		response.sendRedirect("userdetails.jsp");
		
		RequestDispatcher rd = request.getRequestDispatcher("UserServlet");
		rd.forward(request, response);
	}
}
```

**UserServlet.java**

```
package com.example.domain;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class UserServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// get session
		HttpSession session = request.getSession();
		User newuser = (User) session.getAttribute("newuser");

		// get list of users from session
		List<User> users = (ArrayList<User>) session.getAttribute("users");
		
		if (newuser != null) {
			System.out.println("new user found in session");
			// add new user to list
			users.add(newuser);
			
			// remove newuser from session
			session.setAttribute("newuser", null);
		}

//		// add list to request
//		request.setAttribute("users", users);

		// add list to session
		session.setAttribute("users", users);

		// forward to list of users page
		RequestDispatcher rd = request.getRequestDispatcher("userlist.jsp");
		rd.forward(request, response);
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		doGet(req, resp);
	}

}
```

**UserDetails.java**

```
package com.example.domain;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import jakarta.servlet.RequestDispatcher;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

public class UserDetails extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		String next = "home.jsp";
		String name = request.getParameter("name");

		HttpSession session = request.getSession();
		if (session.getAttribute("users") != null) {
			System.out.println("users existed");
			List<User> users = (ArrayList<User>) session.getAttribute("users");
			for (User u : users) {
				if (u.getName().equals(name)) {
					System.out.println("user found");
					System.out.println(u);

					request.setAttribute("user", u);
					next = "userdetails.jsp";
					break;
				}
			}
		}

		RequestDispatcher rd = request.getRequestDispatcher(next);
		rd.forward(request, response);
	}
}
```

## Deployment Descriptor

**web.xml**

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="https://jakarta.ee/xml/ns/jakartaee" xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd" version="6.0">
  <servlet>
    <description></description>
    <display-name>LoginServlet</display-name>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>com.example.login.LoginServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/LoginServlet</url-pattern>
  </servlet-mapping>
  <display-name>MyApp</display-name>
  <welcome-file-list>
    <welcome-file>login.jsp</welcome-file>
  </welcome-file-list>
  <servlet>
    <description></description>
    <display-name>LogoutServlet</display-name>
    <servlet-name>LogoutServlet</servlet-name>
    <servlet-class>com.example.login.LogoutServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>LogoutServlet</servlet-name>
    <url-pattern>/LogoutServlet</url-pattern>
  </servlet-mapping>
  <servlet>
    <description></description>
    <display-name>AddUser</display-name>
    <servlet-name>AddUser</servlet-name>
    <servlet-class>com.example.domain.AddUser</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>AddUser</servlet-name>
    <url-pattern>/AddUser</url-pattern>
  </servlet-mapping>
  <servlet>
    <description></description>
    <display-name>UserServlet</display-name>
    <servlet-name>UserServlet</servlet-name>
    <servlet-class>com.example.domain.UserServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>UserServlet</servlet-name>
    <url-pattern>/UserServlet</url-pattern>
  </servlet-mapping>
  <servlet>
    <description></description>
    <display-name>UserDetails</display-name>
    <servlet-name>UserDetails</servlet-name>
    <servlet-class>com.example.domain.UserDetails</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>UserDetails</servlet-name>
    <url-pattern>/UserDetails</url-pattern>
  </servlet-mapping>
</web-app>
```
