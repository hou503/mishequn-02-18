---
title: How the browser works
date: 2020-10-10 22:15:03
tags: Browser
category: Browser
---
## For applications, the browser has always been important
In 1995, Netscape Inc. rose to prominence with the release of the "Netscape Browser", after which it attempted to develop a web operating system that relied on the browser. This attracted Microsoft's attention and vigilance, so in the same year Microsoft released Windows 95 and bundled with IE, which was a great success, and by 2002, Microsoft had already taken 80% of the browser market share.

This monopoly was not broken until 2008, when Chrome was introduced, completely overturning the previous browser's architecture and design, dominating in speed and security, and increasing its market share dramatically. At the end of 2010, Google also introduced a web operating system, ChromeOS.

As you can see, the browser has been important since its inception, and that importance continues to grow. I've identified three broad evolutionary paths from the browser's history that will hopefully give you an idea of what current web applications can do and what new areas they can be applied to in the future.

The first is the Webification of applications. With the spread of cloud computing and the rapid development of HTML5 technology, more and more applications are shifting to browser/server (B/S) architecture, which makes the browser increasingly important.

The second one is the mobility of Web applications. Although there are still problems to be solved at the technical level, Google has introduced PWA scheme to integrate the advantages of Web and local applications. PWA, by the way, is also something I'm personally looking forward to.

The third one is Web OS. In my opinion, the Web OS has two meanings: one is to use Web technology to build a pure operating system, such as ChromeOS; the other is to move the underlying structure of the browser in the direction of the OS architecture, which will involve many changes in the context of the overall architectural evolution.

Chrome is evolving towards SOA, and in the future, many modules will be provided as services for upper-level applications.
Introducing support for multiple programming languages in the browser, such as the newly supported WebAssembly.
Simplify the rendering process, making it more straightforward and efficient.
Increased support for system equipment features.
Support for the development of complex web projects.
In other words, the browser has gradually evolved into an "operating system" on top of the operating system.

## Why do I need to learn how browsers work?

1. Accurately assess the feasibility of web development projects
With the tremendous richness of Web features and increased browser performance, more and more projects can be developed using the Web. Understanding how the browser works allows you to make more accurate decisions about whether or not you can develop projects using the Web.

For example, last year I worked on a gym virtual trainer project that was very time-intensive, with a lot of high speed rendering of animations and fast interaction with the scene. If I used traditional C++ to develop the interface, it would be impossible to deliver on time, and the maintenance would be very troublesome. So I decided to use a Web solution to develop the interface because it would reduce the development cost and shorten the delivery cycle. In the end, I realized the early delivery of the project with this solution, and the result was very satisfactory.

For this example, I think the best thing I did was to choose the right solution, but on the other hand, if I didn't know anything about browsers and HTML5, it would have been easy for me to abandon the optimal solution.

2. look at the page from a higher dimension
As a qualified developer, you'll also have an important skill: the ability to think about page performance from a user experience perspective. Let's look at a few common UX metrics below.

When a user requests a website and doesn't see critical content within 1 second, the user gets the feeling that the task is interrupted.
When a user clicks on some button, if it doesn't respond within 100ms, the user experiences a delay.
If the animation in the Web does not reach 60fps, the user will experience a stutter in the animation.
Here the page load time, the user interaction feedback time, and the number of frames in the web animation all determine the smoothness of the user experience, and ultimately the effectiveness of the user experience. In a world where user experience is especially important, we must be able to effectively address these experience issues to avoid irreparable damage to our products.

Often, however, these metrics are the result of a complex set of factors. If you want to develop smooth pages or diagnose performance problems in web pages, you need to understand how URLs become pages, and only after you understand this can you locate problems or write efficient code from a global perspective.

You can of course think of the browser as a black box, where you type in a URL on the left, and after the black box process, the right side returns what you expect. If you don't know anything about black boxes, you can still write front-end code and use a lot of best-practice strategies to optimize your code, just as you can write applications on an operating system without understanding how it works.

But if you understand how the black box works, it's a different story. You can look at your project from a higher dimension and quickly locate areas of the project that don't make sense with a full view. For example, the display of the first screen involves DNS, HTTP, DOM parsing, CSS blocking, JavaScript blocking, and other technical factors, and failure to address one of these can result in latency across the page.

If you understand how the browser works, you will be able to string these knowledge points together, and eventually form your own knowledge system, and practice your ability to think and solve problems like an expert.

3. grasp the essence in the fast-paced technology iteration
From 2011 to the present, front-end technology has exploded, with new technologies coming out all the time, and I consider Node.js to be one of the core drivers of front-end development. I think Node.js is one of the core driving forces behind the development of the front-end. The server program!

Although Node.js has only been around for a short time, a huge ecosystem has already formed around it. At the same time, the front-end ecosystem is thriving with new standards and technologies.

Why has Node.js grown so fast? The fundamental reason is that the browser features and the entire front-end development environment are not enough to support the growing demand, so "change" is the main theme of this period. This change has directly expanded the knowledge radius of front-end engineers, which has resulted in many front-end development engineers becoming stack-busters.

Although front-end technology is changing fast, I think there is a bigger opportunity here, and those who can catch the change quickly will be able to reap the dividends of this wave of change.

I believe that as script execution efficiency improves, page rendering performance improves and the development tool chain improves, the next front end will enter a relatively smooth phase. In layman's terms: when the core technology is sufficient to support the core requirements, then the front-end ecology will enter a relatively stable state.

If you understand the working mechanism of the browser, then you can sort out the development of front-end technology, more profound understanding of the current technology, you will also be clear about its shortcomings, as well as the direction of evolution. So let's look at how front-end technology has evolved to address these core requirements.

The first is the issue of script execution speed. For example, the problem of JavaScript design flaws and execution efficiency can be addressed in two ways.

Keep revising and updating the language itself so that you should be aware of the need for ES6, ES7, ES8, or TypeScript to appear. Such revisions are minimal changes to the current ecosystem, so it's easier to implement.
WebAssembly is compiled by a compiler, so it's small, fast, and can dramatically improve language execution, but it takes a long time to improve the language itself, and to build the ecosystem.
Next is the front-end modular development. For example, with the Web applications in various areas of in-depth, Web engineering complexity is also increasing, which generates the need for modular development, so the corresponding WebComponents standard. The familiar React and Vue are adapting incrementally to the WebComponents standard, and the best practices of the various front-end frameworks will in turn influence the WebComponents standard.

If you understand how the browser works, you'll have a better understanding of the technologies involved in WebComponents, such as Shawdow DOM, HTML Templates, and so on.

Finally, there is the issue of rendering efficiency. Again, if you understand the browser's rendering process, you should know that there are still major flaws in the current rendering of pages, and then you'll know how to avoid these problems and develop more efficient web applications. At the same time, the Chrome team is working on improving these flaws, such as LayoutNG, a next-generation layout solution in development, and Slim Paint, a render thinning solution that aims to make rendering easier and more efficient.

As you can see, the factors behind these changes are triggered by the realities of current technology constraints, so understanding how the browser works puts you on a higher plane of understanding the front end.

