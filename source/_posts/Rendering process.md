---
title: Rendering process
date: 2020-11-07 21:45:12
tags: Browser
category: Browser
---

In chronological order of rendering, the pipeline can be divided into the following sub-stages: building the DOM tree, style calculation, layout stage, layering, drawing, blocking, rasterization, and compositing. It's quite extensive, and I'll devote two articles to explaining each of these sub-stages in detail. Next, as you introduce each phase, you should focus on the following three things.

- Begin each sub-stage with its own inputs.
- Each sub-stage then has its own processing.
- Ultimately each sub-stage will generate output content.
- Understanding these three parts will give you a clearer understanding of each sub-stage.

## Building the DOM tree
Why build a DOM tree? This is because the browser can't understand and use HTML directly, so it needs to be converted into a structure that the browser can understand - the DOM tree.

We also need to briefly explain what a tree is, but to make it more intuitive, you can look at some of the tree structures I've drawn below.

![](1.jpg)

To understand the DOM tree more visually, you can open Chrome's Developer Tools, select the "Console" tab to open the console, and then type "document" into the console and enter "Enter" to see a complete DOM tree structure as shown below.

![](2.jpg)


As you can see, the DOM is almost identical to HTML, but unlike HTML, the DOM is a tree-like structure stored in memory that can be queried or modified in JavaScript.

Let's see how you can modify the DOM in JavaScript by typing in the console.

```
document.getElementsByTagName("p")[0].innerText = "black"
```
The purpose of this line of code is to change the content of the first <p> tag to a black

After executing a piece of JavaScript code that modifies the first <p> tag, the content of the DOM's first p node is successfully modified, as well as the content of the page.

Well, now we've generated the DOM tree, but we still don't know the style of the DOM node, and to get the DOM node to have the correct style, we need style calculations.

## Recalculate Style
The purpose of the style calculation is to figure out the specific style of each element in the DOM node, which can be done in three roughly steps.

1. converting the CSS into a structure that the browser can understand.

![](3.jpg)

As you can see from the figure, there are three main sources of CSS styles.

External CSS files referenced by links
CSS within the <style> tag
CSS embedded in the element's style attribute
Like HTML files, these plain-text CSS styles are not directly understood by the browser, so when the rendering engine receives the CSS text, it performs a conversion operation, converting the CSS text into a structure that the browser can understand - styleSheets.

To better understand the structure, you can view it in the Chrome console by typing document.styleSheets into the console, and you'll see the structure as shown below.

![](4.jpg)

As you can see, this style sheet contains many styles, including styles from all three sources. Of course, the structure of the style sheet is not the focus of today's discussion, you just need to know that the rendering engine will convert all the retrieved CSS text to data in the styleSheets structure, and that the structure has both query and modification capabilities, which will provide the basis for later style operations.

Converting property values in style sheets to standardize them
Now that we've transformed our existing CSS text into a browser-understandable structure, the next step is to standardize its attribute values.

To understand what attribute value standardization is, you can look at a CSS text like this.

```css
body { font-size: 2em }
p {color:blue;}
span  {display: none}
div {font-weight: bold}
div  p {color:green;}
div {color:red; }
```
As you can see in the above CSS text there are a lot of attribute values, such as 2em, blue, bold, these types of values are not easily understood by the rendering engine, so all the values need to be converted to standardized calculated values that the rendering engine can easily understand, this process is attribute value standardization.

What do the standardized attribute values look like?
![](5.jpg)

3. calculate the specific style of each node in the DOM tree.
Now that the attributes of the style have been standardized, the next step is to calculate the style attributes for each node in the DOM tree, how do you do this?

This brings us to CSS inheritance rules and cascading rules.

The first is CSS inheritance, where each DOM node contains the style of its parent node. This may sound abstract, but let's look at a concrete example of how a style sheet like this is applied to a DOM node.

The final effect of this style sheet applied to the DOM node is shown below.

![](6.jpg)

It can be seen from the diagram that all child nodes inherit the style of their parent nodes. For example, if the font-size of the body node is 20, then the font-size of all nodes below the body node is equal to 20.

This screen presents a wealth of information and can be roughly described as follows.

First of all, you can select the style of the element you want to view (located in area 2 of the figure), click on the corresponding element in the first area of the figure to view the style of that element. For example, the element we selected here is the <p> tag, which is located under the path html.body.div.
Secondly, you can check the source of the style from the style source (in area 3 in the figure) to see if it is from the style file or from the UserAgent style sheet. If you don't provide any styles, the UserAgent style will be the default.
Lastly, you can see how the style inheritance process works by looking at regions 2 and 3.
These are some of the features of CSS inheritance, the style calculation process will be based on the inheritance of the DOM nodes to calculate the node style.

The second rule in the style calculation process is style cascading. Cascading is a fundamental feature of CSS, it is an algorithm that defines how to merge attribute values from multiple sources. It is central to CSS, and is emphasized by the fact that CSS is called Cascading Style Sheets. We won't go into too much detail about the rules of cascading here, there is a lot of information on the web, you can search and learn on your own.

In short, the purpose of the style calculation phase is to calculate the style of each element in the DOM node, which needs to comply with the CSS rules of inheritance and cascading. The final output of this phase is the style of each DOM node, which is stored in the ComputedStyle structure.

If you want to know the final computed style of each DOM element, you can open Chrome's Developer Tools, select the first "element" tab and then select the "Computed" sub-tab, as shown below.

![](7.jpg)

The red box above shows the ComputedStyle value of the html.body.div.p tag. If you want to see which element, just click on the corresponding tag on the left.

Layout stage
Now, we have the DOM tree and the styles of the elements in the DOM tree, but that's not enough to display the page, because we don't yet have information about the geometric position of the DOM elements. The next step is to calculate the geometry of the visible elements in the DOM tree, and we call this calculation process layout.

Chrome has two tasks to do in the layout phase: creating the layout tree and calculating the layout.

1. creating the layout tree
You may have noticed that the DOM tree also contains a lot of invisible elements, such as head tags, and elements that use the display:none attribute. So before we display them, we will additionally build a layout tree containing only the visible elements.

To build the layout tree, the browser does roughly the following.

Iterate through all visible nodes in the DOM tree and add these nodes to the layout.
Nodes that are not visible are ignored by the tree, such as all the content below the head tag, or the element body.p.span, which is not wrapped into the tree because its attribute contains `dispaly:none`.

2. layout calculation
Now we have a complete layout tree. So the next step is to calculate the coordinate positions of the layout tree nodes. The process of calculating the layout is very complex, so we'll skip it here until I go into more detail in later chapters.

When the layout operation is performed, the result of the layout calculation is rewritten back into the tree, so the tree is both input and output content, which is an irrational place to be in the layout phase, since the input content is not clearly separated from the output content. To address this, the Chrome team is refactoring the layout code with a next-generation layout system called LayoutNG, which attempts to more clearly separate input and output, thus making the newly designed layout algorithm simpler.



