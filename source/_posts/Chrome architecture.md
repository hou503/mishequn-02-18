---
title: Chrome architecture
date: 2020-10-02 21:35:08
tags: Browser
category: Browser
---

## Processes and threads
However, before I introduce processes and threads, I need to explain what parallel processing is, because if you understand the concept of parallel processing, then understanding the relationship between processes and threads becomes much easier.
## What is parallel processing?
Parallel processing in computers is the processing of multiple tasks at the same time, for example, we want to calculate the values of the following three expressions and display the results.
```js
A = 1+2
B = 20/5
C = 7*8
```
In writing the code, we can break the process down into four tasks.

- Task 1 is to calculate A=1+2.
- Task 2 is to calculate B=20/5.
- Task 3 is to calculate C=7*8.
- Task 4 is to display the result of the last calculation.
Normally the program can be handled in a single thread, that is, in four steps to execute each of these four tasks in order.

What happens if it is multi-threaded? We just need to divide into "two steps": the first step, use three threads to execute the first three tasks at the same time; the second step, and then execute the fourth display task.

In a comparative analysis, you will see that it takes four steps to execute in a single thread, but only two steps to execute in multiple threads. Therefore, using parallel processing can greatly improve performance.

## Threads vs. Processes
Multiple threads can handle tasks in parallel, but threads cannot stand alone, they are started and managed by processes. So what is a process again?

A process is a running instance of a program. To elaborate, when you start a program, the operating system creates a memory for the program to store code, running data, and a main thread to execute tasks.


Threads are dependent on processes, and the use of multi-threaded parallel processing in processes can improve computational efficiency.

In summary, the relationship between processes and threads has the following four characteristics. 1.

1. an error in the execution of any thread in the process will cause the entire process to crash.

We can simulate the following scenario.

```js
A = 1+2
B = 20/5
C = 7*8
```
I modified the above three expressions slightly, and when calculating the value of B, I changed the denominator of the expression to 0. When the thread executes to B = 20/0, because the denominator is 0, the thread will execute an error, which will cause the whole process to crash, and of course the other two threads will execute with no result.
2. sharing data between threads in the process.

Threads can read and write to the process's public data.

3. When a process closes, the operating system will reclaim the memory occupied by the process.

When a process exits, the operating system will reclaim all the resources requested by the process; even if any thread leaks memory due to improper operation, the memory will be properly reclaimed when the process exits.

For example, the previous Internet Explorer browser supported many plug-ins, and these plug-ins could easily lead to memory leaks, which means that as long as the browser is on, the memory usage may increase, but when the browser process is closed, all this memory will be reclaimed by the system.

4. Processes are isolated from each other's content.

Process isolation is a technique used to protect the processes in an operating system from interfering with each other, so that each process can only access the data that it occupies, thus avoiding the situation where process A writes data to process B. Process isolation is a technique used to protect the processes in an operating system from interfering with each other. It is because the data between processes is strictly isolated, so if a process crashes or hangs, it will not affect other processes. If you need to communicate data between processes, you need to use a mechanism for inter-process communication (IPC).

The era of the single-process browser
Once we understand processes and threads, let's look at the architecture of a single-process browser together. As the name implies, a single-process browser is one in which all of the browser's functional modules, including the network, plug-ins, JavaScript runtime, rendering engine, and pages, are running in a single process. 

So many functional modules running in a single process is one of the main factors that make single-process browsers unstable, unwieldy and insecure. I'm going to analyze each of the reasons why these problems occur.

Problem 1: Instability
Early browsers needed plug-ins for powerful features such as web video and web games, but plug-ins were the most error-prone module and ran within the browser process, so an accidental crash of one plug-in could cause the entire browser to crash.

In addition to plugins, the rendering engine module is also unstable, and often complex JavaScript code can cause the rendering engine module to crash. As with plugins, a crash of the rendering engine can cause the entire browser to crash.

Problem 2: Sluggishness
As you can see from the above "Single Process Browser Architecture Diagram", all the page rendering modules, JavaScript execution environment and plugins are running in the same thread, which means that only one module can be executed at a time.

For example, the following infinite loop script.

```js
function freeze() {
	while (1) {
		console.log("freeze");
	}
}
freeze();
```
What do you think would happen if you let this script run in a single-process browser page?

Since this script is infinite loop, when it executes, it takes over the entire thread, which results in other modules running in the thread not having a chance to be executed. Since all the pages in the browser are running in the thread, they don't get a chance to execute their tasks, which causes the entire browser to become unresponsive and cluttered. I'll go into more detail about this in a later module on the page's event loop system.

In addition to the aforementioned scripts or plug-ins that can make a single-process browser stutter, memory leaks on a page are also a major cause of single-process slowdowns. Usually the kernel of the browser is very complex, running a more complex page and then close the page, there will be a situation where the memory can not be fully recycled, which leads to the problem that the longer you use, the higher the memory usage, the slower the browser will become.

Problem 3: Insecurity
This can still be explained in terms of both plugins and page scripts.

Plugins can be written in code such as C/C++, and they allow you to access any resource on your operating system, so when you run a plugin on a page, it means that the plugin can fully operate your computer. If it's a malicious plugin, it can release viruses, steal your account password and cause security issues.

As for page scripts, they can gain system access through browser vulnerabilities, and these scripts can also do malicious things to your computer after gaining system access, which can also cause security issues.

These were the characteristics of browsers at the time, unstable, unwieldy, and insecure. It's an unpleasant past that you may not have experienced, but imagine a scenario where you are opening multiple pages in your browser and suddenly one page crashes or becomes unresponsive, followed by the entire browser crashing or becoming unresponsive, and then you realize that the email page you wrote to your boss has disappeared with it, and you feel as devastated as the page?

the era of multi-process browsers
The good news is that modern browsers have solved these problems, how? That brings us to our "multi-process browser era".

Let's first look at how to solve the instability problem. Since the processes are isolated from each other, when a page or plugin crashes, it affects only the current page process or plugin process and does not affect the browser and other pages, which is a perfect solution to the problem that a page or plugin crash can cause the entire browser to crash, which is also known as instability.

Next, let's take a look at how the unwieldy issue is resolved. Again, JavaScript runs in the rendering process, so even if JavaScript blocks the rendering process, it only affects the currently rendered page, not the browser and other pages, because the scripts for the other pages are running in their own rendering process. So when we run the above dead-loop script again in Chrome, only the current page will not respond.

The solution for memory leaks is even easier, because when you close a page, the entire rendering process is closed, and any memory used by that process is reclaimed by the system, thus easily solving the memory leak problem for browser pages.

Finally, let's look at how the two security issues above are resolved. Chrome locks the plug-in and rendering processes inside the sandbox, so that even when the rendering process is running, it can't write any data to your hard drive or read any data in sensitive locations such as your documents or desktop. If a malicious program executes inside a process or plug-in process, the malicious program will not be able to break out of the sandbox and gain access to the system.

Well, after analyzing the early days of Chrome, I'm sure you've already understood the need for a multi-process architecture for your browser.

## Current multi-process architecture

The latest Chrome includes: 1 main browser process, 1 GPU process, 1 NetWork process, multiple rendering processes, and multiple plug-in processes.
Let's analyze the functions of each of these processes.

Browser process. It is responsible for displaying the interface, user interaction, child process management, and providing storage and other functions.
Rendering process. By default, Chrome creates a rendering process for each tab. For security reasons, the rendering processes are run in sandbox mode.
GPU Processes. Actually, when Chrome was first released, there was no GPU process. The GPU was originally intended to be used for 3D CSS, but then the web, and Chrome's UI interface, opted for GPU rendering, making GPU's a common browser requirement. Finally, Chrome also introduced GPU processes to its multi-process architecture.
Web processes. It used to run as a module inside the browser process, but only recently has it become a separate process.
Plugin process. It is mainly responsible for the running of the plug-in, because the plug-in is prone to crash, so it needs to be isolated by the plug-in process to ensure that the crash of the plug-in process will not affect the browser and the page.
At this point, you should now be able to answer the question mentioned at the beginning of the article: why are there 4 processes when only 1 page is open? This is because opening a page requires at least 1 network process, 1 browser process, 1 GPU process, and 1 rendering process, 4 in total, plus 1 plug-in process if the page is running a plugin.

However, there are two sides to every coin, and while the multi-process model improves browser stability, fluidity, and security, it also inevitably brings up some issues.

Higher resource usage. Because each process will contain a copy of the public infrastructure (e.g., the JavaScript runtime environment), this means that the browser will consume more memory resources.
More complex architecture. High coupling between browser modules and poor scalability can lead to an architecture that is already difficult to adapt to new demands.
For both of these issues, the Chrome team was looking for a resilient solution that would address both the high resource consumption and the complex architecture.

## Future service-oriented architecture
To address these issues, in 2016, the official Chrome team designed the new Chrome architecture using the idea of a Services Oriented Architecture (SOA). This means that Chrome's overall architecture will move in the direction of the "Services Oriented Architecture" used by modern operating systems, and the various modules will be reorganized into separate services, each of which can run in a separate process and access the services ( (Service) must use defined interfaces to communicate via IPC to build a more cohesive, loosely coupled, easy-to-maintain and scalable system that better achieves Chrome's goals of simplicity, stability, speed, and security.

