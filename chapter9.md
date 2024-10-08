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
