# Asynchronous Processing

Web containers in application servers normally use a server thread per client request. Under heavy load conditions, containers need a large number of threads to serve all the client requests. 

Scalability limitations include running out of memory or exhausting the pool of container threads. 

To create scalable web applications, you must ensure that no threads associated with a request are sitting idle, so the container can use them to process new requests.

There are two common scenarios in which a thread associated with a request can be sitting idle.

- The thread needs to wait for a resource to become available or process data before building the response. For example, an application may need to query a database or access data from a remote web service before generating the response.
- The thread needs to wait for an event before generating the response. For example, an application may have to wait for a JMS message, new information from another client, or new data available in a queue before generating the response.

These scenarios represent blocking operations that limit the scalability of web applications. 

Asynchronous processing refers to assigning these blocking operations to a new thread and retuning the thread associated with the request immediately to the container.

## Asynchronous Processing in Servlets

Java EE provides asynchronous processing support for servlets and filters. 

If a servlet or a filter reaches a potentially blocking operation when processing a request, it can assign the operation to an asynchronous execution context and return the thread associated with the request immediately to the container without generating a response. 

The blocking operation completes in the asynchronous execution context in a different thread, which can generate a response or dispatch the request to another servlet.

To enable asynchronous processing on a servlet, set the parameter asyncSupported to true on the @WebServlet annotation as follows:

![image](https://github.com/user-attachments/assets/30cc6c7e-13c7-4b70-9209-e2bd7ed879aa)

The javax.servlet.AsyncContext class provides the functionality that you need to perform asynchronous processing inside service methods. 

To obtain an instance of AsyncContext, call the startAsync() method on the request object of your service method; for example:

![image](https://github.com/user-attachments/assets/800522cb-d406-4c4e-be9b-6e439304bfbd)

This call puts the request into asynchronous mode and ensures that the response is not committed after exiting the service method. 

You have to generate the response in the asynchronous context after the blocking operation completes or dispatch the request to another servlet.

## Functionality Provided by the AsyncContext Class

| Method Signature | Descriptions |
|------------------|--------------|
| void start(Runnable run) | The container provides a different thread in which the blocking operation can be processed.<br/>You provide code for the blocking operation as a class that implements the Runnable interface. You can provide this class as an inner class when calling the start method or use another mechanism to pass the AsyncContext instance to your class.|
| ServletRequest getRequest() | Returns the request used to initialize this asynchronous context. In the example above, the request is the same as in the service method.<br/>You can use this method inside the asynchronous context to obtain parameters from the request. |
| ServletResponse getResponse() | Returns the response used to initialize this asynchronous context. In the example above, the response is the same as in the service method.<br/>You can use this method inside the asynchronous context to write to the response with the results of the blocking operation. |
| void complete() | Completes the asynchronous operation and closes the response associated with this asynchronous context.<br/>You call this method after writing to the response object inside the asynchronous context. |
| void dispatch(String path) | Dispatches the request and response objects to the given path.<br/>You use this method to have another servlet write to the response after the blocking operation completes. |

## Example

Create the JSP view and Shout Servlet.

**index.jsp**

![image](https://github.com/user-attachments/assets/d460817a-757e-40f0-b7cc-48fd66d5bcfa)

**ShoutServlet.java**

![image](https://github.com/user-attachments/assets/fa4a1e38-df09-4908-9aff-f4cb6718df09)

Run the application and test it in multiple browsers/tabs. Post another shout by filling the form. Notice only one browser updates.

![image](https://github.com/user-attachments/assets/e1ff9203-19c0-4af1-a5b6-7c851173784d)

## Add the Asynchronous Servlet and JavaScript code

**index.jsp - Using AJAX for get method on ShoutServlet**

Add the AJAX call to wait until a message is posted and print it on the page by adding the highlighted code.

![image](https://github.com/user-attachments/assets/c3f42a11-170c-4609-ae0b-d3706216c68d)

The code you added will continuously request the Shout Servlet for a new message and append the response of the Servlet to the content div.

You need to add the asynchronous support to the Servlet and implement the message notification.

Open the ShoutServlet class and add the highlighted code.

**ShoutServlet.java - Asynchronous Servlet for notifications**

![image](https://github.com/user-attachments/assets/b9706ffc-9588-499a-b95a-a1148b706bdd)

When the Servlet receives a "get" Request, it starts an AsyncContext by calling: request.startAsync(request, response). This will notify the Web Container that at the end of the request call it should free the handling thread and leave the connection open so that other thread writes the response and end the connection.

The next thing you did in the code is adding the AsyncContext to a List so we can get all waiting contexts in the doPost method.

Then you modified the doPost method to get a safe copy of the list of AsyncContext.Then you cleared the common list to prevent a pending request to be notified twice. Finally for all the AsyncContexts queued you wrote the message to their responses and completed those requests by calling asyncContext.complete()

Test your application by running it.

Right-click on your project name and Select Run.

A browser will open. Open another browser window and move them side by side.

Fill the form with your name and type a message as Your shout. Click SHOUT.

You should see your shout posted on both browser windows.

![image](https://github.com/user-attachments/assets/2d491d72-ee8a-4154-ad1b-3fad5c7be4f5)

The page still refreshes when you post a message. To make the page refresh partially when you submit a shout, add the highlighted code to the index.jsp file:

![image](https://github.com/user-attachments/assets/af3a2e7d-0297-4df9-8c36-f8323680ba93)
