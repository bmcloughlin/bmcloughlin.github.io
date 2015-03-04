---
layout: post
title: ASP.NET MVC 5 Pipeline Extensibility
---

We have all looked at those large data flow diagrams that try to depict the lifecycle of a request and subsequent response in ASP.NET MVC applications and some of us will have  dug deeper than others.  Over a series of posts I hope to examine the processing pipeline that the request and response move through with a focus on the extensibility points that are provided to developers.  It is important for development teams to understand all of the possible extension points of the framework in order to make the correct implementation choices and to build structured, maintainable and of course testable code.

This processing pipeline can be decomposed into seven steps which are:

###Route identification

*HTTP requests to IIS are intercepted by the HTTP module URLRoutingModule which traverses the routing table attempting to match the requested url with a route. Once a route has been located the IRouteHandler class associated with this route is used to provide the  URLRoutingModule with an IHttpHandler which will be used to handle the request.*

**Extensiblity**  
*IRouteConstraint* : Allows developers to extend the standard route matching mechanism.

*IRouteHandler* : By default routes will use the MvcRouteHandler implementation but developers can use this interface to implement a custom handler which essentially will take the matched request out of the Mvc pipeline.


###Controller Instantiation

*Assuming the request is to remain in the MVC pipeline the handler MvcHandler will be used.  This handler uses an IControllerFactory to locate the required controller.  Mvc includes a default implementation of this interface to instantiate the contoller which looks for objects implementing IController with the postfix "controller" and instantiates them using reflection.  To use dependency injection here an IoC implementation of IControllerFactory is required.* 

**Extensiblity**  
*IControllerFactory* : Allows developers to introduce new mechanisms for controller instantiation.

###Action Selection

*Now that the pipeline has created the controller it will find the correct action to execute and this is done using the default implementation of IActionInvoker which is AsyncControllerActionInvoker.  Initially the action to be invoked must be identified and this will simply be matching the method and action with the same name through the ActionNameSelectorAttribute and where the ActionNameSelectorAttribute returns more that one result the correct action is chosen using ActionMethodSelectorAttribute.*

**Extensiblity**  
*IActionInvoker* :
*ActionNameSelectorAttribute* :
*ActionMethodSelectorAttribute* :


###Access Verification

*The next step in the pipeline is to authenticate and to authorise the caller to insure they can execute the chosen action.  Authentication is done using an implementation of IAuthenticationFilter.  If the request is authenticated sucessfully the pipeline will authorise the request using any IAuthorisationFilter objects that are configured. Authorize is a well known implementation of the IAuthorisationFilter interface.  Both IAuthenticationFilter and IAuthorisationFilter objects are known as "Action Filters" and are discovered via the IFilterProvider*

**Extensiblity**  
*IAuthorisationFilter* : Allows developers to create custom authorisation filters
*IAuthenticationFilter* : Allows developers to create custom authentication filters
*IFilterProvider": Allows developers to introduce new mechanisms for discovering filters including IoC.


###Model Binding

*Once the method is identified and access verified if it has parameters, these parameters must be populated which is done via the model binder (IModelBinder) and during this process validation of each parameter is performed, this validation can be extended using the ModelValidatorProvider.  Validation can be further extended the model binding process by implementing IValidatableObject in the object being bound.*

**Extensiblity**  
*IModelBinder* :
*ModelValidatorProvider* :
*IValidatableObject* :


###Action Execution

*If no Action Filters (IActionFilter) prevent  the pipeline from proceeding the parameters are now populated with the values from the Model Binding Step and the action is then executed.  Also there maybe other filters that are applied  before the response is returned. Filters that execute after the action itself are IActionFilter and IResultFilter*

**Extensiblity**  
*IActionFilter* :
*IResultFilter* :


###Result or View Execution 
..

