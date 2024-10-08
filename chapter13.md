# Security

JavaServer Pages and servlets make several mechanisms available to Web developers to secure applications. Resources are protected declaratively by identifying them in the application deployment descriptor and assigning a role to them.

Several levels of authentication are available, ranging from basic authentication using identifiers and passwords to sophisticated authentication using certificates.

## Role Based Authentication

The authentication mechanism in the servlet specification uses a technique called role-based security. 

The idea is that rather than restricting resources at the user level, you create roles and restrict the resources by role.

You can define different roles in file tomcat-users.xml, which is located off Tomcat's home directory in conf. An example of this file is shown below.

![image](https://github.com/user-attachments/assets/1feea15c-6c27-4c3e-a20a-ced7e490530c)

This file defines a simple mapping between username, password, and role. Notice that a given user may have multiple roles; for example, username = "both" is in the "tomcat" role and the "role1" role.

Once you have identified and defined different roles, role-based security restrictions can be placed on different Web Application resources by using the <security-constraint> element in web.xml file available in the WEB-INF directory.

Following is a sample entry in web.xml

![image](https://github.com/user-attachments/assets/00c50b34-aacd-408c-802e-b524d7f97b11)

The above entries would mean.

- Any HTTP GET or POST request to a URL matched by /secured/* would be subject to the security restriction.
- A person with the role of a manager is given access to the secured resources.
- The login-config element is used to describe the BASIC form of authentication.

If you try browsing any URL including the /security directory, the following dialog box will be displayed asking for username and password. If you provide a user "admin" and password "secret", then you will have access on the URL matched by /secured/* as we have defined the user admin with manager role who is allowed to access this resource.

## Form Based Authentication

When you use the FORM authentication method, you must supply a login form to prompt the user for a username and password. Following is a simple code of login.jsp. This helps create a form for the same purpose.

![image](https://github.com/user-attachments/assets/d4b6fc91-ec9d-4b3a-81e4-282d85b72652)

Here you have to make sure that the login form must contain the form elements named j_username and j_password. The action in the <form> tag must be j_security_check. POST must be used as the form method. At the same time, you will have to modify the <login-config> tag to specify the auth-method as FORM.

![image](https://github.com/user-attachments/assets/67f07c81-911f-4e8f-b3f5-2538eddc86f8)

Now when you try to access any resource with URL /secured/*, it will display the above form asking for the user id and password. When the container sees the "j_security_check" action, it uses some internal mechanism to authenticate the caller.

If the login succeeds and the caller is authorized to access the secured resource, then the container uses a session-id to identify a login session for the caller from that point on. The container maintains the login session with a cookie containing the session-id. The server sends the cookie back to the client, and as long as the caller presents this cookie with subsequent requests, then the container will know who the caller is.

If the login fails, then the server sends back the page identified by the form-error-page setting.

Here, j_security_check is the action that applications using form based login have to specify for the login form. In the same form, you should also have a text input control called j_username and a password input control called j_password. When you see this, it means that the information contained in the form will be submitted to the server, which will check name and password. How this is done is server specific.

## Programmatic Security in a Servlet/JSP

The HttpServletRequest object provides the following methods, which can be used to mine security information at runtime.

| Method | Descriptions |
|--------|--------------|
| String getAuthType() | The getAuthType() method returns a String object that represents the name of the authentication scheme used to protect the Servlet. |
| boolean isUserInRole(java.lang.String role) | The isUserInRole() method returns a boolean value: true if the user is in the given role or false if they are not. |
| String getProtocol() | The getProtocol() method returns a String object representing the protocol that was used to send the request. This value can be checked to determine if a secure protocol was used. |
| boolean isSecure() | The isSecure() method returns a boolean value representing if the request was made using HTTPS. A value of true means it was and the connection is secure. A value of false means the request was not. |
| Principle getUserPrinciple() | The getUserPrinciple() method returns a java.security.Principle object that contains the name of the current authenticated user. |

For example, for a JavaServer Page that links to pages for managers, you might have the following code.

![image](https://github.com/user-attachments/assets/46673027-410e-4928-b4c0-6d00785dbe6c)

By checking the user's role in a JSP or servlet, you can customize the Webpage to show the user only the items she can access. If you need the user's name as it was entered in the authentication form, you can call the getRemoteUser method in the request object.

## Example: Programmatic Security in JSP

In this example, we are creating a JSP file Secured.jsp to show the username and role of the logged in user.

**web.xml**

![image](https://github.com/user-attachments/assets/acd41611-e885-4255-bd06-0e4b7db5483d)

**login.jsp**

![image](https://github.com/user-attachments/assets/ab52b7ff-eb07-4d85-aabf-c35334b9c722)

**failure.jsp**

![image](https://github.com/user-attachments/assets/00f1ff28-19c9-44b2-8330-f44aff058012)

**secured.jsp**

![image](https://github.com/user-attachments/assets/2af2e8a1-a725-4e44-b7e3-c55b72c46c65)

**Output**

Run your secured.jsp file and enter admin credentials as we configure it for admin role, you will get the following output:

![image](https://github.com/user-attachments/assets/76c39da2-11df-4ec9-bfad-43551c13a142)

