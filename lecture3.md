# Week 3 - JavaBeans and Sessions

## JSP and JavaBeans

Request/Response &rightarrow; JSP &rightarrow; JavaBeans &rightarrow; Business Processing

**Using JavaBeans** - Allows us to encapsulate reusable functionality in a "bean". A bean is a normal Java class that follows some rules (called Design Patterns). One basic design pattern is for "properties" which is using a getter and setter methods. JavaBeans are not a client-side component model (they are not EJBS, Enterprise JavaBeans). A JavaBean is a class that conforms to the following conventions:

- provides a public constructor with no parameter as well as with parameter(s)
- provides a public get and set methods (to access private fields).
- implements the Serialization interface. Serialization is the process of converting an object into a stream of bytes in order to store the object or transmit it to memory, a database, or a file. Classes `ObjectInputStream` and `ObjectOutputStream` are high-level streams that contain the methods for serializing and de-serializing an object.

```java
public class Person implements Serializable {
  private String name;

  public Person() {} //default constructor
  public Person(String name) {  //parameterized constructor
    this.name = name;
  }

  public String getName() {  //Getter
    return this.name;
  }
  public void setName() {  //Setter
    this.name = name;
  }
  //Method naming convention is important!!
}
```

```jsp
//Using beans
<% uts.wsd.User user = new uts.wsd.User(email, name, password); %>
-------- OR -----
<%@ page import="uts.wsd.*" %>
<% User user = new User(email, name, password); %>

//Setting properties
<% user.setEmail("jone@doe.com"); %>
//Getting properties
<%= user.getEmail() %>
```

- Using beans `<jsp:useBean id="user" class="uts.wsd.User" scope="page" />`
  - `uts.wsd.User user = new uts.wsd.User()`
- Setting Properties `<jsp:setProperty name="user" property="email" value="jone@doe.com" />`
  - `user.setEmail("jone@doe.com")`
- Getting properties `<jsp:getProperty name="user" property="email" />`
  - `user.getEmail()`

**JSP Scopes** - JSP allows us to save data into a scope.

| Scope       | Where saved                                                 |
| ----------- | ----------------------------------------------------------- |
| page        | Only for the current page, destroyed when we leave the page |
| request     | kept while request is active                                |
| session     | saved in the current session.                               |
| application | saved for the whole applicaton                              |

**EL and JavaBeans** - Expression Language also can access JavaBeans:

```jsp
//Instead of
<jsp:useBean id="user" class="uts.wsd.User" />
<jsp:getProperty name="user" property="email"/> //this invokes user.getEmaill()

//use
${user.email}
```

## Sessions

Many websites use **session-based** interactions. HTTP is a stateless protocol so to maintain state in a web application we use **Session** and **Cookies**. A cookie is a small piece of tet stored by a user's browser. Cookies store session information on the client (a unique identifier is stored on the client side called session id). A session is a serverside storage of information that is desired throughout the user's interactionwith the web site. The web application pairs this session id with it's internal data and retrieves the stored variables for use by the requested page.

**JSP Session** - A session is an implicit object associated with a visitor.

```java
//Data can be put in the session
Person p = new Person("John Smith");
<% session.setAttribute("theperson",p);%>

//Data can be retrieved from it
Person p = (Person) session.getAttribute("theperson");

<p> The person's name: <%= p.getName() %> </p>
```

The content of the session will be remembered until the user closes his/her web browser or until our web application explicitly clears the session. To end an application session : `<% session.invalidate(); %>` inside logout.jsp

## JSP deployment

A single JSP page is pretty simple but a complex web application is more complex and includes static HTML files, static images, JSP files, separate Java classes, CSS stylesheets etc. So, we need to package web applications for deployment.

Web applications are packaged as **WAR** (Web Application aRchive) files. WAR files can include all of the file types plus a **deployment descripter** to specify configuration ssettings and make the application as container-independent as possible. A WAR file can be deployed into any compliant web container or JSP/Servlet container. _Apache Tomcat is a web container._
