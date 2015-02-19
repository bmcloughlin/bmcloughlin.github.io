---
layout: post
title: ASP.NET MVC 5 Pipeline Extensibility
---

We have all looked at those large data flow diagrams that try to depict the lifecycle of a request and subsequent response in ASP.NET MVC application and some of us will have  dug deeper than others.  Over a series of posts I hope to examine the processing pipeline that the request and response move through with a focus on the extensibility points that are provided to developers.  It is important for development to understand all of the possible extension points of the framework in order to make the correct implementation choices and to build structured, maintainable and of course testable code.

This processing pipeline is generally broken up into four parts which are:

1. Route identification
2. Controller Instantiation
3. Action Execution
4. Result Execution (View)


