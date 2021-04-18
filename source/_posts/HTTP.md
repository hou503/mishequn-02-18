---
title: HTTP
date: 2020-10-25 20:38:14
tags: HTTP
category: HTTP
---

## HTTP request flow on the browser side

1. Constructing the request

First, the browser builds the request line information, and when it is built, the browser is ready to initiate a network request.

```
GET /index.html HTTP1.1
```
2. Find cache
Before actually initiating a web request, the browser looks in the browser cache to see if the file to be requested is available. The browser cache is a technique for saving a copy of a resource locally for direct use in the next request.

When the browser finds that a copy of the requested resource is already in the browser cache, it intercepts the request, returns a copy of that resource, and simply ends the request without going to the source server to re-download it. The benefits of this are.

Relieves server-side stress and improves performance (less time spent acquiring resources).
For websites, caching is an important part of achieving fast resource loading.
Of course, if the cache lookup fails, you'll be in the web request process.
3. Prepare the IP address and port
But, not so fast, before we can understand web requests, we need to look at the relationship between HTTP and TCP. Since the browser uses HTTP as the application layer protocol to encapsulate the text of the request and TCP/IP as the transport layer to send it to the network, the browser needs to establish a connection to the server via TCP before the HTTP work can begin. This means that the content of HTTP is implemented through the data transfer phase of TCP, and you can better understand the relationship between the two by looking at the diagram below.

![](1.jpg)

The first step is to request DNS to return the IP address of the domain name, but the browser also provides DNS data caching service, if a domain name has already been resolved, the browser will cache the resolution results for the next query, this will also reduce a network request.

Once you have the IP address, the next step is to get the port number. Typically, the HTTP protocol defaults to port 80 if the URL doesn't specify a port number.

4. Waiting for the TCP queue
Chrome has a mechanism that allows up to 6 TCP connections to be established on the same domain at the same time, so if 10 requests occur on the same domain at the same time, 4 of them will be queued until the ongoing request is completed.

Of course, if the current number of requests is less than 6, it will go straight to the next step to establish a TCP connection.

5. Establishing a TCP connection
After waiting in line, you can finally shake hands with the server happily, and the browser establishes a connection to the server over TCP before the HTTP work begins.
6. Sending HTTP requests
Once the TCP connection is established, the browser can communicate with the server. It is during this communication that the data in the HTTP is transferred.

You can see how the browser sends the requested information to the server in the figure below.

![](2.jpg)
First the browser sends a request line to the server, which includes the request method, the request URI (Uniform Resource Identifier), and the HTTP version protocol.

Sending a request line tells the server what resource the browser needs, and the most common request method is Get, for example, by typing the time.geekbang.org domain name directly into the browser's address bar, which tells the server to Get its home page resource.

Another common request method is POST, which is used to send some data to the server, such as logging into a website, which requires sending user information to the server via the POST method. If the POST method is used, then the browser also has to prepare data to send to the server, and the data prepared here is sent through the request body.

After the browser sends the request line command, it also sends some other information in the form of a request header, which tells the server some basic information about the browser. This includes, for example, information about the operating system used by the browser, the kernel of the browser, the domain name of the current request, the cookie information of the browser, and so on.

## Server Side Processes HTTP Request Flow
1. Returning requests
Once the server has finished processing the request, it is ready to return the data to the browser. You can view the returned request data through the utility curl by typing the following command on the command line.

```
curl -i  https://bing.com/
```

Note that the -i is added to return the response line, response header, and response body data, which returns the results shown below.

![](2.jpg)
First the server will return the response line, including the protocol version and status code.

But not all requests can be processed by the server, so what about some of the messages that cannot be processed or that are processed incorrectly? The server tells the browser the result of its processing by the status code of the request line, such as.

The most commonly used status code is 200, indicating successful processing.
If the page is not found, a 404 is returned.
There are many types of status codes, so I won't go into too much detail here, there is a lot of information online that you can look up and learn on your own.

Subsequently, just as the browser sends a request header with the request, the server sends a response header to the browser with the response. The response header contains some information about the server itself, such as the time the server generates the returned data, the type of data returned (JSON, HTML, streaming, etc.), and the cookies the server wants to save on the client.

After sending the response header, the server can continue to send the response body of the data, usually, the response body contains the actual content of the HTML.

This is how the server responds to the browser.

2. Disconnection
Normally, once the server returns the requested data to the client, it has to close the TCP connection. However, if the browser or server includes the following in its header information.

```
Connection:Keep-Alive 
```
Then the TCP connection will remain open after sending, so the browser can continue to send requests over the same TCP connection. Keeping the TCP connection open eliminates the time it takes to establish a connection for the next request and speeds up resource loading. For example, if you initialize a persistent connection to a Web page with embedded images from the same Web site, you can reuse that connection to request other resources without having to make a new TCP connection.
3. Redirection
At this point it looks like the request process is almost over, but there is one more thing you should know, for example, when you open geekbang.org in your browser, you'll see that the page you end up on is https://www.geekbang.org.

The reason the two URLs are different is that there is a redirect involved. As before, you can still use curl to see what a request to geekbang.org will return.

Enter the following command at the console.
```
curl -I geekbang.org

```
Note that the parameter entered here is -I, which is not the same as -i. -I means that you only need to get the response header and response line data, but not the response body data, and the final returned data is shown below.
![](3.jpg)
As you can see, the status code returned by the response line is 301, which tells the browser that I need to redirect to another URL, and the URL to be redirected is contained in the Location field of the response header. The redirect execution process. This explains why you type in geekbang.org and end up with https://www.geekbang.org.

## Why do many sites open quickly the second time?
If the page opens quickly the second time, the main reason is that some time-consuming data is cached during the first page load.

So, what data gets cached? From the core request path described above, we can see that two pieces of data, DNS cache and page resource cache, are cached by the browser. Among them, DNS cache is relatively simple, it is mainly associated with the corresponding IP and domain name in the local browser, here we do not do too much analysis.

Let's focus on the browser resource cache, and here's how the cache is processed.

![](4.jpg)

When the server returns the HTTP response header to the browser, the browser uses the Cache-Control field in the response header to set whether to cache the resource or not. Usually, we also need to set a cache expiration time for this resource, which is set by the Max-age parameter in Cache-Control.

```
Cache-Control:Max-age=2000
```
This means that if the cached resource is requested again before the cache expires, it will be returned to the browser directly, but if the cache expires, the browser will continue to make web requests with the following message in the HTTP request header.

However, if the cache has expired, the browser will continue to make web requests with the following HTTP request header.

```
If-None-Match:"4f80f-13c-3a1xb12a"
```
When the server receives the request header, it determines if the requested resource has been updated based on the value of the If-None-Match.

If there are no updates, a 304 status code is returned, which is the equivalent of the server telling the browser, "This cache can continue to be used, so I won't send you data repeatedly this time."
If the resource has an update, the server returns the latest resource directly to the browser.

Briefly, many websites are able to open in seconds on a second visit because they cache many of their resources locally, and the browser cache saves time by responding to requests directly using a local copy without generating a real web request. At the same time, the DNS data is also cached by the browser, which eliminates the need for DNS lookups.

## How is login status maintained?
With the above, you've learned how caching works. Let's take a look at how the login status is maintained.

The user opens the login page, fills in the username and password in the login box, and clicks the OK button. Clicking the OK button triggers the page script to generate the user login information and then calls the POST method to submit the user login information to the server.
After receiving the information submitted by the browser, the server will query the backend to verify if the user login information is correct, if so, it will generate a string to show the user's identity and write the string to the Set-Cookie field of the response header as shown below, then send the response to the browser.

```
Set-Cookie: UID=3431uad;
```
After receiving the response header from the server, the browser starts to parse the response header, and if it encounters a response header that contains a Set-Cookie field, the browser will save this field information locally. If the response header contains a Set-Cookie field, the browser will save this information locally, for example, keeping UID=3431uad local.
When the user visits again, the browser makes an HTTP request, but before the request is made, the browser reads the previously saved cookie data and writes the data into the Cookie field in the request header, and then the browser sends the request to the server.

```
Cookie: UID=3431uad;
```
When it finds the information containing UID=3431uad, the server queries the background and determines that the user is logged in, then generates page data containing the user information and sends the generated data to the browser.
After receiving the page data of the current user, the browser can display the logged in status information of the user correctly.
Now, you can see from this flow that the browser page status is achieved by using cookies. cookie flow can be seen in the following figure.
![](5.jpg)

Simply put, if a server-side response is sent with a field for Set-Cookie in the response header, the browser will keep the contents of that field local. The next time the client sends a request to the server, the client will automatically add a cookie value to the request header before sending it out. When the server finds the cookie, it checks which client sent the connection request and compares it with the server's records to get the user's status information.

## Sum up
The HTTP request in a browser goes through eight stages from initiation to completion: building the request, finding the cache, preparing the IP and port, waiting for the TCP queue, establishing the TCP connection, initiating the HTTP request, processing the request by the server, returning the request by the server, and disconnecting the connection.


