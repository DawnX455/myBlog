---
layout: post
title:  "Introduction to Java Web"
date:   2021-08-01 22:40:45 -0500
categories: Java Web
---
# What is Web Application?

A web application, like Google mail,  is application software running on a web server. User can use web browsers to access the web application with active network connection. Web application uses client-server modeled structure, where a server needs to manage requests for users.

## Difference Between Web application and Website

Website is a collection of related web pages that contains images, text, autio, video, etc. Users can read websites by using a browser(chrome, firefox).

It seems similar with web application, but web application focuses on services for users while websites plays a role as displaying.

| Web Application                                 | Website                         |
| ----------------------------------------------- | ------------------------------- |
| interaction with users                          | display static content          |
| users read content and also manipulate the data | only read the content           |
| mostly requires authentication                  | authentication is not necessary |



# Accessing a Web Application

![](/Users/xujingwen/Library/Application Support/typora-user-images/image-20210731014827644.png)

The figure above shows the process of accessing a web application:

- Client sends a HTTP request to the web application server(Tomcat).
-  If the client requests for static resourses like images or text, the Web server will find the resourses through file system; if the request is for dynamic resouces, eg. login, the JSP/Servlet will interact with database to deal with the request.
- Web server sends the HTTP reponse to client.

Now, let's talk more details about this process!

# Web Server vs. Web Application Server

A web server delivers static web content-eg., HTML pages, files, images, video.

A web application server can also deliver the web page, but the primary job is to enable interaction between clients and server code. The code in the server-side is often called *business logic* , which can generate and deliver dynamic content.

## Tomcat 

The Apache Tomcat is an open source application server that executes Java Servlets and serves Java EE applications.

### Install Tomcat

1. Download `zip` or `tar.gz` from Tomcat official website https://tomcat.apache.org/download-80.cgi

<img src="/Users/xujingwen/Library/Application Support/typora-user-images/image-20210731032635896.png" alt="image-20210731032635896" style="zoom: 33%;" />

2. Unzip Tomcat and enter the `bin/` directory

   ``` shell
   cd apache-tomcat-9.0.50/bin
   ```

3. Run startup.sh 

   ``` shell
   ./startup.sh 
   ```

4. Visit http://localhost:8080/

   If we see the website as below, it means that the Apache Tomcat runs successfully.

   <img src="assets/images/%image-20210731033515655.png" alt="image-20210731033515655" style="zoom: 25%;">

5. Stop Tomcat

   ``` shell
   ./shutdown.sh
   ```

### Tomcat configuration

Tomcat configuration file `server.xml` is in `conf/` directory.

1. Port number

   The default port number of Tomcat is `8080`, we can change it with modifying the `port=` in `Connector` part. 

   Some common port number: 

   - HTTP: 8080
   - HTTPS: 443
   - MySQL: 3306

   ``` xml
   <Connector port="8080" protocol="HTTP/1.1"
              connectionTimeout="20000"
              redirectPort="8443" />
   ```

2. Host name:

   The default host name is `localhost`, and the default store place of web application is `webapps`.

   ```xml
   <Host name="localhost"  appBase="webapps"
         unpackWARs="true" autoDeploy="true">
   ```

## Web Application Directory Structure

This is the directory structure of Apache Tomcat example.

<img src="/Users/xujingwen/Library/Application Support/typora-user-images/image-20210731040632198.png" alt="image-20210731040632198" style="zoom:50%;" />

Now, let's get some explaination.

```java
--webapps ：The web directory in Tomcat(apache-tomcat-9.0.50/webapps)
	-ROOT
	-examples : The name of website
		- WEB-INF
			-classes : Java Program
			-lib：Library 
			-web.xml ：Configuration file
		- index.html : Default homepage
		- static : Store static resources
            -css
            	-style.css
            -js
            -img
         -.....
```



## HTTP 

### What is HTTP?

HTTP (hypertext transport protocol) is an application-layer protocol for transmitting hypermedia documents, eg., HTML, image, music, videos, etc. HTTP supports the communication between clients and web servers, by sending the request from client-side, and the response from the server-side. HTTP is stateless, meaning that server does not keep any state between two requests.

#### Components in HTTP-based System

#### 	<img src="/Users/xujingwen/Library/Application Support/typora-user-images/image-20210731054944546.png" alt="image-20210731054944546"  />

- Client: user-agent, is any tool that acts on the behalf of the user. This role is primarily performed by the Web browser. 
- Web Server: deals with the HTTP request from the client, and sends HTTP response to client.
- Proxy:  receives requests from client and forwards them to server. It is on the application layers, and can perform functions like:
  - Caching
  - Filter: exam web traffic to identify suspicious content.
  - Load balancing: allow multiple servers to serve the difference requests.
  - Authentication: control access to resources.
  - Logging: allow the storage of historical information.



### HTTP Request

The development of HTTP can be divided into two eras:

​	HTTP/1.0: the client side (browser) keeps a short connection with server side. After the server responses, the connection will be closed. This means that every time the browser sends a request, it has to construct the TCP connection with the server. It is very inefficient since opening and closing connection will waste time, for example, if a browser wants to view a website with many pictures, it has to build a lot of TCP connections to download them.

​	HTTP/1.1: supports the persistent connection, so multiple HTTP requsts and responses can be sent by one TCP connection, reducing the consume and  delay on opening and closing TCP connections. In the example mentioned above, if we use HTTP/1.1, then the HTTP request and response for pictures can be dealed with one TCP connection, although each pair (HTTP request, HTTP response) is to serve one picture. HTTP/1.1 also allows clients not to wait on one response to make next request, while the server must send the response by the ordering of requests.

Example of HTTP message:

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

```html
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html
```

Explainations:

1. request header:

   Method of request:`GET`, `POST`,  `HEAD`, `DELETE`, `PUT`...

   - GET: requests for the required resources. This method can bring less parameters,  the size of GET request is limited, and the request content will be shown in URL in the browser, so it is not secure.
   - POST: requests that a web server accepts the data enclosed in the body of the request message, primarily for creating/ updating a resource. The parmaters with POST method are not limited, and it's data won't be shown in URL in the browser.