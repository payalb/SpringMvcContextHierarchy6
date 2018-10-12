That’s an interface extending ApplicationContext with a contract for accessing the ServletContext.

: the root web application context is simply a centralized place to define shared beans.

The root web application context is managed by a listener of class org.springframework.web.context.ContextLoaderListener, which is part of the spring-web module.By default, the listener will load an XML application context from /WEB-INF/applicationContext.xml.  However, those defaults can be changed

We can specify an alternate location of the XML context configuration with the contextConfigLocation parameter:

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/rootApplicationContext.xml</param-value>
</context-param>
Or more than one location, separated by commas:

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/context1.xml, /WEB-INF/context2.xml</param-value>
</context-param>
We can even use patterns:


<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>/WEB-INF/*-context.xml</param-value>
</context-param>

So 1 root context with all xmls defined using the above pattern

Let’s see, for example, how to use Java annotations configuration instead.

We use the contextClass parameter to tell the listener which type of context to instantiate:

<context-param>
    <param-name>contextClass</param-name>
    <param-value>
        org.springframework.web.context.support.AnnotationConfigWebApplicationContext
    </param-value>
</context-param>
Every type of context may have a default configuration location. In our case, the AnnotationConfigWebApplicationContext does not have one, so we have to provide it.

We can thus list one or more annotated classes:

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>
        com.baeldung.contexts.config.RootApplicationConfig,
        com.baeldung.contexts.config.NormalWebAppConfig
    </param-value>
</context-param>
Or we can tell the context to scan one or more packages:

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>org.baeldung.bean.config</param-value>
</context-param>
And, of course, we can mix and match the two options.


Using webapplicatioNInitializer

XmlWebApplicationContext rootContext = new XmlWebApplicationContext();
Then, in the second line, we tell the context where to load its bean definitions from. Again, setConfigLocations is the programmatic analogous of the contextConfigLocation parameter in web.xml:

rootContext.setConfigLocations("/WEB-INF/rootApplicationContext.xml");

Finally, we create a ContextLoaderListener with the root context and register it with the servlet container. As we can see, ContextLoaderListener has an appropriate constructor that takes a WebApplicationContext and makes it available to the application:

The WebApplicationInitializer class that we’ve seen earlier is a general-purpose interface. It turns out that Spring provides a few more specific implementations, including an abstract class called AbstractContextLoaderInitializer.

Its job, as the name implies, is to create a ContextLoaderListener and register it with the servlet container.

We only have to tell it how to build the root context:


public class AnnotationsBasedApplicationInitializer 
  extends AbstractContextLoaderInitializer {
  
    @Override
    protected WebApplicationContext createRootApplicationContext() {
        AnnotationConfigWebApplicationContext rootContext
          = new AnnotationConfigWebApplicationContext();
        rootContext.register(RootApplicationConfig.class);
        return rootContext;
    }
}
Here we can see that we no longer need to register the ContextLoaderListener, which saves us from a little bit of boilerplate code.

Spring MVC applications have at least one Dispatcher Servlet configured (but possibly more than one, we’ll talk about that case later). This is the servlet that receives incoming requests, dispatches them to the appropriate controller method, and returns the view.

Each DispatcherServlet has an associated application context. Beans defined in such contexts configure the servlet and define MVC objects like controllers and view resolvers.

the root context is the parent of every dispatcher servlet context. Thus, beans defined in the root web application context are visible to each dispatcher servlet context, but not vice versa.

https://www.baeldung.com/spring-web-contexts


servletContext.addListener(new ContextLoaderListener(rootContext));

----------------------------------------------
jsp: property file : internationalization??

accept-language: header


?language=en
param: language

LocaleResolver: session/cookie

displayStudent

//2 language: english | language
Name: 

2) Login/Logout: Login only during checkout is necessary

cart into the db: cart information in browser cache/ session level
checkout: logged in: save the cart in the db
Logout: session clear
annotation: @SessionAttributes

3) spring mvc: jsp | bootstrap, jquery, js
database: 

how many tables
product
user information: username, first name, last name, address, mobile number
payment 
cart information
login: username, password, role
cart
add products/ remove products
booking history

database?? 
normalization:

Oracle/ mysql: rdbms: relational database management system
logical layer on top of physical layer
relational database: ?
tables| relationships : foreign key/ primary key

nosql db
	document  (mongodb)

Spring mvc: 

noteapd
cheatsheet: spring mvc
database design

yesterday:

1) custom validation: 1) annotation : @interface {target, retention, Constraint: ConstraintValidator: logic for validation}
2) interceptor? HandlerInterceptor: | register the interceptor: WebMvcConfigurer
3) exception handling? @ControllerAdvice, @ExceptionHandler, try/catch, mapping for it in webmvcconfigurer:addViewController
/* registry.addViewController("/error").setViewName("error"); */
4) internationalization: 
5) Session handling?? @SessionAttributes("cart"): 

	Product: 
	Product: 
	Product: 

		@ModelAttribute("cart") Product product)
	
6) Method annotated with @ModelAttribute: It shud be called before it calls any controller. Will set the model for that controller
headerMsg, footerMsg

7) Method annotated with  @InitBinder: to be run before binding
8) @PathVariable , @RequestParam, Model, ModelMap, ModelAndView, @ModelAttribute
9) Property file: PropertyPlaceholderConfigurer, @PropetySources
10) SessionAttributes: Map

Advanced : 
1) Context hierarchy
java config 	xml config

spring config file: java	
spring :	java	xml
xml

web.xml/Class: WebApplicationInitializer
DispatcherServlet | ContextLoaderListener
WebApplicationContext| ApplicationContext
ContextHierarchy
Multiple DispatcherServlet??

ApplicationContext: spring core container

WebApplicationContext:spring container: web application:
?? spring mvc

Root context: Parent: load any bean : it will be available to all child contexts
Child context:


WebApplicationContext:
1) xml: ClasspathXmlWebApplicationContext
2) java: AnnotationConfigWebApplicationContext

Case1:

2 spring config file
spring-db.xml:database
spring-mvc.xml: mvc code: interceptor, view resolver, controller
spring-core.xml: property: beans

web.xml
<servlet>
<servlet-name>servlet1</>
<class>DispatcherServlet</class>
<url-pattern> /


servlet1-servlet.xml: dispatcher servlet: initializing ur webapplicationContext: child context


WebapplicationContext: applicationContext: applicationContext.xml
						java: ??
WebApplicationInitializer
root context
	: applicationContext.xml: XmlWebApplicationContext
	?? ContextLoaderListener: root context file
	
child context







 


	


