# Activity 2

## Complete the Registration Form process

In this activity, we are going to update the Registration Form to include input text to collect user's information.

- Name
- Email
- Address
- Phone number

Add additional button to submit the form.

The updated index.html file will look like below.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Registration Form</title>
</head>
<body>
	<h1>Registration Form</h1>
	<form action="processform" method="POST">
		<p>
			Name: <input type="text" name="username">
		</p>
		<p>
			Email: <input type="text" name="useremail">
		</p>
		<p>
			Address: <input type="text" name="useraddress">
		</p>
		<p>
			Phone: <input type="text" name="userphone">
		</p>
		<input type="submit" value="Submit Form">
	</form>
	<form action="LifeCycleServlet">
		<input type="submit" value="invoke life cycle servlet">
	</form>
</body>
</html>
```

Re-run the project and launch the index.html form.

![image](https://github.com/user-attachments/assets/6cb1e33d-a162-4afb-b1bf-f97836886f6d)

Now, we need a servlet to process the registration form. Stop the application and add a new servlet to the project. Update the servlet with below codes.

```
package com.example.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ProcessForm
 */
public class ProcessForm extends HttpServlet {
	private static final long serialVersionUID = 1L;

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// get user input from registration form
		String name = request.getParameter("username");
		String email = request.getParameter("useremail");
		String address = request.getParameter("useraddress");
		String phone = request.getParameter("userphone");

		// prepare to display result on page
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<h2>Registration Completed</h2>");
		out.println("<p>Here are your details:<p>");
		out.println("<ul>");
		out.println("<li>Name: " + name + "</li>");
		out.println("<li>Email: " + email + "</li>");
		out.println("<li>Address: " + address + "</li>");
		out.println("<li>Phone: " + phone + "</li>");
		out.println("</ul>");

		out.println("<a href='index.html'>Back</a>");
	}

}
```

Re-run the project and test the registration form.

![image](https://github.com/user-attachments/assets/be4b4b3c-4d2a-4765-a473-c2164b8e2d8a)

Upon submission, you will see the result page like below.

![image](https://github.com/user-attachments/assets/e108b431-c126-44e5-84c8-2d77cfa39805)

End of activity.

