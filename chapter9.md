# Event Listeners

The servlet specification includes the capability to track key events in your Web applications through event listeners. 

This functionality allows more efficient resource management and automated processing based on event status. The following sections describe servlet event listeners.

## Event Categories and Listener Interfaces

There are two levels of servlet events:

- Servlet context-level (application-level) event

  This event involves resources or state held at the level of the application servlet context object.

- Session-level event

  This event involves resources or state associated with the series of requests from a single user session; that is, associated with the HTTP session object.

Each of these two levels has two event categories:

- Lifecycle changes
- Attribute changes

You can create one or more event listener classes for each of the four event categories. A single listener class can monitor multiple event categories.

Create an event listener class by implementing the appropriate interface or interfaces of the javax.servlet package or javax.servlet.http package. Table below summarizes the four categories and the associated interfaces.

| Event Category | Event Description | Java Interface |
|----------------|-------------------|----------------|
| Servlet context lifecycle changes | Servlet context creation, at which point the first request can be serviced.<br/>Imminent shutdown of the servlet context. | ServletContextListener |
| Servlet context attribute changes | Addition of servlet context attributes.<br/>Removal of servlet context attributes.<br/>Replacement of servlet context attributes. | javax.servlet. ServletContextAttributeListener |
| Session lifecycle changes | Session creation.<br/>Session invalidation.<br/>Session timeout. | javax.servlet.http. HttpSessionListener. |
| Session attribute changes | Addition of session attributes.<br/>Removal of session attributes.<br/>Replacement of session attributes. | javax.servlet.http. HttpSessionAttributeListener |

## Typical Event Listener Scenario

Consider a Web application comprising servlets that access a database. A typical use of the event listener mechanism would be to create a servlet context lifecycle event listener to manage the database connection. This listener may function as follows:

1. The listener is notified of application startup.
2. The application logs in to the database and stores the connection object in the servlet context.
3. Servlets use the database connection to perform SQL operations.
4. The listener is notified of imminent application shutdown (shutdown of the Web server or removal of the application from the Web server).
5. Prior to application shutdown, the listener closes the database connection.

## Event Listener Declaration and Invocation

Event listeners are declared in the application web.xml deployment descriptor through <listener> elements under the top-level <web-app> element. Each listener has its own <listener> element, with a <listener-class> subelement specifying the class name. Within each event category, event listeners should be specified in the order in which you would like them to be invoked when the application runs.

After the application starts up and before it services the first request, the servlet container creates and registers an instance of each listener class that you have declared. For each event category, listeners are registered in the order in which they are declared. Then, as the application runs, event listeners for each category are invoked in the order of their registration. All listeners remain active until after the last request is serviced for the application.

Upon application shutdown, session event listeners are notified first, in reverse order of their declarations, then application event listeners are notified, in reverse order of their declarations.

![image](https://github.com/user-attachments/assets/be27684a-0fba-4259-ab3c-ff5e4ddf6e7a)

Assume that MyConnectionManager and MyLoggingModule both implement the ServletContextListener interface, and that MyLoggingModule also implements the HttpSessionListener interface.

When the application runs, both listeners are notified of servlet context lifecycle events, and the MyLoggingModule listener is also notified of session lifecycle events. For servlet context lifecycle events, the MyConnectionManager listener is notified first, because of the declaration order.

## Event Listener Coding and Deployment Guidelines

Be aware of the following rules and guidelines for event listener classes:

- In a multithreaded application, attribute changes may occur simultaneously. There is no requirement for the servlet container to synchronize the resulting notifications; the listener classes themselves are responsible for maintaining data integrity in such a situation.
- Each listener class must have a public zero-argument constructor.
- Each listener class file must be packaged in the application WAR file, either under /WEB-INF/classes or in a JAR file in /WEB-INF/lib.

## Event Listener Sample

This section provides code for a sample that uses a servlet context lifecycle and session lifecycle event listener. This includes the following components:

- SessionLifeCycleEventExample: This is the event listener class, implementing the ServletContextListener and HttpSessionListener interfaces.
- SessionCreateServlet: This servlet creates an HTTP session.
- SessionDestroyServlet: This servlet destroys an HTTP session.
- index.jsp: This is the application welcome page (the user interface), from which you can invoke SessionCreateServlet or SessionDestroyServlet.
- web.xml: This is the deployment descriptor, in which the servlets and listener class are declared.

## Welcome Page: index.jsp

Here is the welcome page, the user interface that enables you to invoke the session-creation servlet by clicking the Create New Session link, or to invoke the session-destruction servlet by clicking the Destroy Current Session link.

![image](https://github.com/user-attachments/assets/7e08383e-16b7-4599-969f-c3c060342a14)

## Deployment Descriptor: web.xml

The servlets and the event listener are declared in the web.xml file. This results in SessionLifeCycleEventExample being instantiated and registered upon application startup. Because of this, the servlet container automatically calls its methods, as appropriate, upon the occurrence of servlet context or session lifecycle events. 

Here are the web.xml entries:

![image](https://github.com/user-attachments/assets/52d014f0-e093-4336-ae41-8c7ff07a9e75)

## Listener Class: SessionLifeCycleEventExample

This section shows the listener class. Its sessionCreated() method is called by the servlet container when an HTTP session is created, which occurs when you click the Create New Session link in index.jsp. When sessionCreated() is called, it calls the log() method to print a "CREATE" message indicating the ID of the new session.

The sessionDestroyed() method is called when the HTTP session is destroyed, which occurs when you click the Destroy Current Session link. When sessionDestroyed() is called, it calls the log() method to print a "DESTROY" message indicating the ID and duration of the terminated session.

![image](https://github.com/user-attachments/assets/2d2a68ad-8f4b-4c82-b71d-2bc099c2ce6d)

## Session Creation Servlet: SessionCreateServlet.java

This servlet is invoked when you click the Create New Session link in index.jsp. Its invocation results in the servlet container creating a request object and associated session object. Creation of the session object results in the servlet container calling the sessionCreated() method of the event listener class.

![image](https://github.com/user-attachments/assets/89988436-c19e-4bad-9748-ecacc7324f16)

## Session Destruction Servlet: SessionDestroyServlet.java

This servlet is invoked when you click the Destroy Current Session link in index.jsp. Its invocation results in a call to the invalidate() method of the session object. This, in turn, results in the servlet container calling the sessionDestroyed() method of the event listener class.

![image](https://github.com/user-attachments/assets/1468079e-d682-4b0d-b056-0fe179b86dcd)



