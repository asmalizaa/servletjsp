# Servlets

## Servlet API Overview

The Servlet API is a key component of the Java EE (Enterprise Edition) platform, providing a framework for building web applications. It defines the classes and interfaces that developers use to write servlets, which are Java programs that extend the capabilities of a server. Servlets can respond to requests from web clients, typically browsers, and generate dynamic content.

## Key Components of the Servlet API

| Component | Description | Key Methods |
|-----------|-------------|-------------|
| **Servlet Interface** | The Servlet interface is the central abstraction in the Servlet API. All servlets must implement this interface, either directly or, more commonly, by extending classes that implement it. | - init(ServletConfig config): Initializes the servlet. <br/>- service(ServletRequest req, ServletResponse res): Handles requests and generates responses. <br/>- destroy(): Cleans up resources before the servlet is destroyed. <br/>- getServletConfig(): Returns the ServletConfig object. <br/> - getServletInfo(): Returns information about the servlet. |
