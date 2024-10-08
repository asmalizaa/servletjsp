# Decorating Requests and Responses

The introduction of filters in the servlet specification was very exciting as it introduced a powerful intercept model. 

A filter is a Java object that can intercept the HttpServletRequest object before the request reaches the servlet's service method and the HttpServletResponse object after the control leaves the service method. You can do useful things with filters, such as verifying user input and compressing web content. 

But the creative use of filters is hindered by the fact that you cannot change the request parameters of an HttpServletRequest object, because the java.util.Map that contains the parameters in an HttpServletRequest object is immutable. This greatly reduces the usefulness of filters. 

At least half the time, you wish you could change the object you are applying your filter to. Wouldn't it be great if you could, for example, write a filter that trims users' input before the HttpServletRequest object reaches the Struts action servlet? Then you would not need to trim them in your Struts action form's validate methods.

Fortunately, you can use the Decorator design pattern to change the behavior of immutable objects even though you cannot change the objects themselves.

## The Decorator Pattern

With inheritance, you can change the behavior of an object by extending the object's class and therefore override the methods whose behaviors you want to modify. However, inheritance is powerless if the object in question is constructed by another subsystem in your application, such as an object factory or a servlet container.

The Decorator pattern can also be used to add functionality to an existing object or to change the behavior of an object. Rather than extending the class using inheritance, this pattern wraps an object in another object. Figure below shows the UML class diagram of the Decorator pattern.

![image](https://github.com/user-attachments/assets/5429a5e5-7270-431e-b349-6c5bbd38b357)

In figure above, the Component interface defines a component whose implementation is ConcreteComponent. To change the behavior of Component, you can modify ConcreteComponent or extend it. However, if ConcreteComponent comes from a factory, you cannot do this. What you can do is create a decorator that also implements Component. In Figure 1, the decorator is represented by Decorator, which is usually coded as an interface or abstract class. One of the features of the Decorator type is a constructor that accepts a Component object. You pass the object to be decorated to this constructor; in this case, this object is the ConcreteComponent object you obtain from the factory. By assigning the decorated object to a class variable in Decorator, you have access to it from any method in Decorator. This will then enable you to add behavior to the object.

The Decorator type in above figure does not have to be an interface or an abstract class. If your application is not very complex, you can write the implementation as a concrete Decorator class.

As an example, consider a simple messaging application that centers on the Messenger interface and its implementation MessengerImpl. Let's assume that MessengerImpl objects come from a factory, so you cannot modify their behavior. If you need to add or change the functionality of Messenger objects, you can create a MessengerDecorator class. Figure below shows the class diagram for this example.

![image](https://github.com/user-attachments/assets/a2ebfcd7-3fab-4478-a191-3f832cfa9101)

## Example

The Messenger interface.

![image](https://github.com/user-attachments/assets/b5c28ce8-6751-4ca6-bfad-9d96bf74ad99)

The MessengerImpl class.

![image](https://github.com/user-attachments/assets/0cb9ecf9-5748-4834-889c-c172588efae5)

Messenger objects are created by a factory called MessengerFactory.

## The MessengerFactory class

![image](https://github.com/user-attachments/assets/cfe7c762-eb03-467a-90ce-fe7c9fa1e6a9)

This factory sets the initial state, that is, the String returned by getMessage(), of every Messenger object it creates based on some unknown formula. In other words, you cannot create the Messenger object yourself.

In your application, the purpose of having a Messenger object is to pass it to the broadcast() static method of a utility class called Util.

## The Util class

![image](https://github.com/user-attachments/assets/7837608a-d9e3-47a5-8b98-f27369c5b86d)

In your own class, you may have the following code:

![image](https://github.com/user-attachments/assets/3b94bd1e-46cd-4f9e-9291-238189518fb7)

Suppose, you want to modify slightly the message printed by the broadcast() method. You want it to be printed in upper case letters. What can you do? Technically, you could subclass Messenger, instantiate the subclass, and feed the returned object to Util.broadcast(). However, that would be pointless because only the factory object knows how to initialize a Messenger object so that its getMessage() method returns the correct value.

Using the Decorator pattern, you can create a MessengerDecorator class.

## The MessengerDecorator class

![image](https://github.com/user-attachments/assets/216a5d5a-4b4b-4b34-81d3-9086613d9b83)

Because MessengerDecorator implements Messenger, Util.broadcast() will accept an instance of MessengerDecorator. However, MessengerDecorator is not just an implementation; it is a decorator of a MessengerImpl object. For this, there must be a constructor in MessengerDecorator that accepts the Messenger object to be decorated.

As codes below shows, this constructor assigns the argument to a variable. You can now override the getMessage() method in MessengerDecorator so that it prints the message in upper case letters. Because you have the reference to the original Messenger object, you can write your getMessage() method like this:

![image](https://github.com/user-attachments/assets/68f5aea4-a2f3-4c83-8107-be4de5ee9a6d)

The getMessage() method in MessengerDecorator returns the upper case version of the original message.

In your class, you can get a Messenger object as usual and pass the Decorator to Util.broadcast.

![image](https://github.com/user-attachments/assets/80d8fb69-a9b1-4142-baa9-9dea2c9faacd)

Instead of passing the original object to the intended destination, you pass to it a decorator of the object.

## Applying the Decorator Pattern to Servlets

The example with the Messenger class above parallels the ServletRequest objects constructed by servlet containers. Upon receiving an HTTP request, a servlet container creates ServletRequest and ServletResponse objects (an instance of ServletRequestImpl and an instance of ServletResponseImpl, respectively) and passes the two objects to the specified servlet's service method. Now if you can create a decorator for ServletRequest and pass it to the servlet's service method, you will have implemented the Decorator pattern.

It is really easy to implement this pattern with ServletRequest, because the servlet API provides a wrapper class for ServletRequest: ServletRequestWrapper. Figure below represents the UML class diagram of a servlet decorator.

![image](https://github.com/user-attachments/assets/66439bd7-9990-4b87-bf8d-91ddde373db9)

The HTTP version of the class diagram in figure above is shown in the next figure. Don't get confused by the extra classes. Just focus on the three types (HttpServletRequest, HttpServletRequestImpl, HttpServletRequestWrapper) in the box.

![image](https://github.com/user-attachments/assets/2c3098b1-0451-4b4a-9ba7-679b0d4d4d36)

This is a similar situation. You have an implementation of ServletRequest, but it is created by the servlet container. You can decorate these ServletRequest objects using the supplied ServletRequestWrapper.

This pattern is easy and applicable in real-world applications. In fact, it has been applied to a number of high-profile applications, including the following.

- Struts - Struts is one of the most popular framework for developing Java web applications based on the Model-View-Controllers pattern. Struts provides the org.apache.struts.upload.MultipartRequestWrapper class that functions as a ServletRequest wrapper. Designed to handle file upload, MultipartRequestWrapper overrides the getParameter(), getParameterNames(), and getParameterValues() methods.
- Apache Beehive - This open source project, originally part of BEA's WebLogic Workshop, runs on top of Struts and eases the development of web applications and web services. The PageFlowRequestWrapper class in the org.apache.beehive.netui.pageflow.internal package functions as a ServletRequest wrapper that helps in the page flow processing of any Apache Beehive applications.

## Example

**A Trimming Filter**

This section puts the theory into practice by showing an example trimmer servlet filter that illustrates the use of the javax.servlet.http.HttpServletRequestWrapper class as a decorator template for HttpServletRequest objects. In this example, the trimmer filter will trim superfluous whitespace from incoming parameters.

This can be useful in many servlet/JSP applications, including Struts and JavaServer Faces applications. For example, Struts populates an action form by calling the getParameterValues() method on the HttpServletRequest object. By overriding this method in a decorator class, you change the behavior of the current HttpServletRequest object.

To create a decorator for HttpServletRequest, you need to extend HttpServletRequestWrapper and override the methods you want to change. Listing 5 presents the MyRequestWrapper class that trims the return value of the getParameterValues() method.

## HttpServletRequest decorator

![image](https://github.com/user-attachments/assets/e6ba7914-b62a-4d7a-820c-a07df908581b)

## The trimming filter

![image](https://github.com/user-attachments/assets/2b5fd5b5-c9ad-495a-b39c-f27301aef041)
![image](https://github.com/user-attachments/assets/38789b8d-9813-486b-817f-83cf9b1edeff)

The trimmer application uses the trimmer filter to trim user input. To apply the filter, you need the following filter and filter-mapping elements in the web.xml file:

![image](https://github.com/user-attachments/assets/2d650817-a6be-4b1f-a7de-eaef7cbbc021)

To test the filter, invoke the trimmer application and type some values into the form, submit the form, and see how the filter trims the input values. This is a practical, and useful, application of the Decorator pattern.

## Summary

Servlet filters can be used to pre-process and post-process HTTP requests after the invocation of a servlet's service method. This is very promising but the potential uses for filters are limited because you cannot change HttpServletRequest objects.

The Decorator pattern can help. This article demonstrated how to apply the Decorator pattern to "modify" HttpServletRequest objects, thereby making your servlet filters much more useful. In the example trimmer filter, the filter changes user input in the request parameters, something you would not be able to do without decorating the request objects.



