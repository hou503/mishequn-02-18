---
title: From Input URL to Page Presentation
date: 2020-10-31 22:40:22
tags: Browser
category: Browser
---

1. User input
When the user enters a query keyword in the address bar, the address bar determines whether the keyword is the search content or the requested URL.

If it is a search, the address bar will use the browser's default search engine to compose a new URL with the search keyword.
If the content matches the URL rules, such as time.geekbang.org, then the address bar will add a protocol to the content to make it a full URL, such as https://time.geekbang.org, according to the rules.
When the user enters the keyword and types "Enter", the browser enters the following state.

![](1.jpg)

As you can see in the figure, the icon on the tab enters the loading state when the browser first starts to load an address. But at this time, the page in the figure still shows the content of the previously opened page, and is not immediately replaced with a geek time page. This is because you need to wait for the document submission phase before the page content is replaced.

2. the URL request process
Next, the page resource request process takes place. At this point, the browser process sends the URL request to the web process via inter-process communication, and the web process receives the URL request and initiates the actual URL request process here. What does that process look like?

First, the web process looks for whether the resource is cached in the local cache. If there is a cached resource, it returns the resource directly to the browser process; if the resource is not found in the cache, it goes directly to the web request process. The first step before this request is to perform DNS parsing to get the IP address of the server that is requesting the domain name. If the requesting protocol is HTTPS, then a TLS connection is also established.

The next step is to establish a TCP connection to the server using the IP address. After the connection is established, the browser end constructs the request line, request header, and other information, appends data such as cookies related to the domain name to the request header, and then sends the constructed request information to the server.

After receiving the request information, the server generates response data based on the request information and sends it to the web process. Once the network process has received the response line and response header, it begins to parse the response header.

(1) Redirect.

After receiving the response header from the server, the web process starts to parse the response header, if the returned status code is 301 or 302, the server needs the browser to redirect to another URL, the web process reads the redirected address from the Location field of the response header, and then initiates a new HTTP or HTTPS message. request, and everything starts all over again.

For example, we enter the following command in the terminal.

```js
curl -I http://time.geekbang.org/
```
The command curl -I + URL is to receive information about the response header returned by the server. After executing the command, we see the following response header information returned by the server.

![](2.jpg)

As you can see in the diagram, the Geek Time Server converts all HTTP requests into HTTPS requests by redirecting them. This means that when you make a request to the Geek Time server using HTTP, the server will return a response header containing a 301 or 302 status code and fill the Location field of the response header with the HTTPS address, which tells the browser to redirect to the new address.

Now let's make another request for Geek Time using the HTTPS protocol to see what the server's response header information looks like.
```js
curl -I http://time.geekbang.org/
```
![](3.jpg)

As you can see from the image, the server returned a response header with a status code of 200, which is telling the browser that everything is fine and it's ready to move on to the request.

Okay, that's the description of the redirect content. Now you should understand that during navigation, if the server response line status code contains a 301, 302 type of jump information, the browser will jump to the new address to continue navigation; if the response line is 200, then it means that the browser can continue to process the request.

(2) Response data type processing

After processing the bounce information, we continue with the analysis of the navigation flow. the data type of the URL request, sometimes a download type, sometimes a normal HTML page, so how does the browser distinguish between them?

The answer is Content-Type.Content-Type is a very important field in the HTTP header, it tells the browser server what type of response body data is being returned, and then the browser will decide how to display the content of the response body based on the value of the Content-Type.

Let's take the example of Geek Time and see what the Content-Type value returned by the Geek Time website is. Enter the following command in the terminal.
```js
curl -I http://time.geekbang.org/
```

![](4.jpg)

As you can see in the figure, the value of the Content-type field in the response header is text/html, which is what tells the browser that the data returned by the server is in HTML format.

Next, let's use curl to request the address of the Geek Time installation package, as follows.

```js
curl -I https://res001.geekbang.org/apps/geektime/android/2.3.1/official/geektime_2.3.1_20190527-2136_offical.apk

```
![](5.jpg)

From the response header information returned, the value of its Content-Type is application/octet-stream, which shows that the data is of byte stream type, and usually the browser will process the request according to the download type.

It should be noted that if the server does not configure the Content-Type correctly, such as configuring the text/html type to application/octet-stream, the browser may misinterpret the content of the file, for example, it may turn a page that was intended for display into a download file.

Therefore, the subsequent processing flow is also very different for different Content-Types. If the value of the Content-Type field is determined by the browser to be a download type, the request is submitted to the browser's download manager, and the navigation process for the URL request ends there. In the case of HTML, however, the browser will continue the navigation process. Since Chrome's page rendering runs within the rendering process, the next step is to prepare the rendering process.

3. Preparing the rendering process
By default, Chrome assigns a rendering process to each page, which means that a new rendering process is created for each new page that is opened. However, there are some exceptions, and in some cases the browser will have multiple pages running directly in the same rendering process.

And when do multiple pages run in a single rendering process at the same time?

To solve this problem, we need to understand what a same-site is. Specifically, we define "same-site" as the root domain name (e.g., geekbang.org) plus a protocol (e.g., https:// or http://) that includes all the sub-domains and different ports under that root domain name, such as the following three.

```
https://time.geekbang.org
https://www.geekbang.org
https://www.geekbang.org:8080
```
They all belong to the same site because their protocol is HTTPS and the root domain is geekbang.org.

Chrome's default policy is to have one rendering process per tag. However, if a new page is opened from a page that belongs to the same site as the current page, the new page will multiplex the rendering process of the parent page. Officially, this default policy is called process-per-site-instance.

To summarize, the rendering process strategy used to open a new page is.

A separate rendering process is normally used to open new pages.
If you open page B from page A, and both A and B belong to the same site, then page B multiplexes the rendering process from page A. In other cases, the browser process creates a new rendering process for B. The rendering process is not ready yet, because the document data is not yet submitted to the rendering process.
When the rendering process is ready, you can't immediately enter the document parsing state, because the document data is still in the network process, and has not been submitted to the rendering process, so the next step is to submit the document.

4. Submit document
To be clear, the term "document" refers to the data in the response body of a URL request.

The "submit document" message is sent by the browser process. The rendering process receives the "submit document" message and establishes a "pipeline" with the web process to transfer the data.
The rendering process will return a "Submit Confirmation" message to the browser process when the document data is transmitted.
When the browser process receives the "Submit Confirmation" message, it updates the browser interface state, including the security state, the URL of the address bar, the forward and backward history, and the Web page.
5. Rendering phase
Once the document has been submitted, the rendering process starts the page parsing and sub-resource loading. All you need to know is that once the page is generated, the rendering process sends a message to the browser process, which receives the message and stops the loading animation on the tab icon.

## Sum up

The server can control the browser's behavior based on response headers, such as jumping and network data type determination.
Chrome defaults to one rendering process per tab, but if two pages belong to the same site, then the two tabs will use the same rendering process.
The browser's navigation process covers all the intermediate stages from the time the user initiates a request to the time the document is submitted to the rendering process.
If you understand the navigation process, you will be able to link the entire page display process together, which is the key to understanding how the browser works.


