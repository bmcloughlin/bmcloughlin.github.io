---
layout: post
title: ASP.NET MVC 5 Pipeline Extensibility
---

We have all looked at those large data flow diagrams that try to depict the lifecycle of a request and subsequent response in ASP.NET MVC applications and some of us will have  dug deeper than others.  Over a series of posts I hope to examine the processing pipeline that the request and response move through with a focus on the extensibility points that are provided to developers.  It is important for development teams to understand all of the possible extension points of the framework in order to make the correct implementation choices and to build structured, maintainable and of course testable code.

This processing pipeline can be decomposed into four sections which are:

###Route identification

*HTTP requests to IIS are intercepted by the HTTP module URLRoutingModule which traverses the routing table attempting to match the requested url with a route. Once a route has been located the IRouteHandler class associated with this route is used to provide the  URLRoutingModule with an IHttpHandler which will be used to handle the request.*

**Extensiblity**  
- *IRouteConstraint* : Allows developers to extend the standard route matching mechanism
- *IRouteHandler* : By default routes will use the MvcRouteHandler implementation but developers can use this interface to implement a custom handler which essentially will take the matched request out of the Mvc pipeline.


###Controller Instantiation
..
###Action Execution
..
###Result or View Execution 
..

