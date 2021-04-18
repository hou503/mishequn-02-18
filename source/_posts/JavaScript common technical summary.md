
---
title: JavaScript common technical summary
date: 2020-8-1 20:00:00
tags:  JavaScript
category: JavaScript
---
Develop, read, and learn about some of the knowledge points encountered and organized.
# Several implementations of JavaScript 

## Global variable
```JavaScript
let  count = 0;
const  countUp = () =>  count++;
```

## Closure
```JavaScript
// javascript
const  countUp = (() => {
    let  count = 0;
    return () => {
        return ++count;
    };
})();
console.log(countUp()); // 1
console.log(countUp()); // 2
```

## Function property
```JavaScript
// javascript
let  countUp = () => {
    return ++countUp.count;
};
countUp.count = 0;
console.log(countUp()); // 1
console.log(countUp()); // 2
```

# Function properties (TS)
```JavaScript
interface  Counter {
(): void; // Here the structure Counter must be defined to contain a function, which is required to have no parameters and a return value of void, i.e. no return value
count: number; // And the structure must also contain an attribute named count, with a value of type number.
}
const  getCounter = (): Counter  => { // A function is defined here to return this counter.
    const  c = () => { // Define a function with the same logic as in the previous example.
        c.count++;
    };
    c.count = 0; // Add a count attribute to this function with an initial value of 0.
    return  c; // Finally, this function object is returned
};

const  counter: Counter = getCounter(); // Get this counter from the getCounter function
counter();
console.log(counter.count); // 1
counter();
console.log(counter.count); // 2
```

# Front-end voice
Voice broadcast: in the project you need to return the message of the ajax request for voice broadcast, str for the return of the data.

```JavaScript
function  voiceAnnouncements(str){
    var  url = "http://tts.baidu.com/text2audio?lan=zh&ie=UTF-8&text=" + encodeURI(str);
    var  n = new  Audio(url);
    n.src = url;
    n.play();
}
voiceAnnouncements('Hello World!')
```

## Use a Proxy to implement a data binding and listen to the
Proxy Profile.

```JavaScript
let  p = new  Proxy(target, handler);
// `target` Represents the object to which you need to add a proxy.
// `handler` Used to customize the operations in the object.
```

```JavaScript
let  onWatch = (obj, setBind, getLogger) => {
    let  handler = {
        get(target, property, receiver) {
            getLogger(target, property)
            return  Reflect.get(target, property, receiver);
        },

        set(target, property, value, receiver) {
            setBind(value);
            return  Reflect.set(target, property, value);
        }
    };
    return  new  Proxy(obj, handler);
};


let  obj = { a:  1 }
let  value
let  p = onWatch(obj, (v) => {
    value = v
}, (target, property) => {
    console.log(`Get '${property}' = ${target[property]}`);
})
p.a = 2  // bind `value` to `2`
p.a  // -> Get 'a' = 2
```

# Let's talk about closing the bag.

## Definition of first-class citizenship
  In programming languages, first-class citizens can be used as function arguments, as function return values, and as assignments to variables. For example, strings are first-class citizens in almost all programming languages, strings can be used as function arguments, strings can be used as function return values, and strings can be assigned to variables. For various programming languages, functions are not necessarily first-class citizens anymore, such as versions before Java 8. For JavaScript, functions can be assigned to variables, as function arguments, or as function return values, so functions are first-class citizens in JavaScript.

## Dynamic vs. static scopes

Note that it says scopes, not types.
  If the scope of a language is a static scope, then the referential relationships between symbols can be clearly determined at compile time based on the program code, and will not change at run time. Where a function is declared, it has the scope of the scope in which it is located. It can access which variables, then bound to these variables, at runtime has always been able to access these variables. That is, static scopes can be determined by the program code, at compile time can be completely determined. Most languages are statically scoped.
  Dynamic Scope (DSC). That is, variable references are not tied to variable declarations at compile time. It is dynamically looking for a variable with the same name in the running environment at runtime. The bash scripting language, as used in macOS or Linux, is dynamically scoped.

## Closure
  The inherent contradiction of a closure is between the runtime environment and the scope at definition time. So we take the variables we need in our internal environment, package them up to the closure function, and it can access them at any time.

  The concept of a closure is a challenge for beginners. In fact, closure is the function in the static scope of the variables accessed in the life of the long, forming a function can be accessed by the data alone. Because the data can only be accessed by the function closed package, so it also has a package of information, hiding the internal details of the characteristics.

## Closures and Object-Orientation
  Does that sound familiar? Encapsulation, sealing the data and the operations on the data together, that's object-oriented programming! A closure can be viewed as an object. On the other hand, can an object be viewed as a closure as well? An object's attributes, which can also be viewed as environment variables that are exclusive to the method, must also have a lifespan to ensure that they can always be accessed properly by the method.

## Implementation of closed packets
Functions have to become first-class citizens of JavaScript. That is, they have to be able to assign functions to variables like normal values, which can be passed as arguments to other functions, and which can be used as the return value of the function.
To have the inner function always access variables in its environment, regardless of whether the outer function exits or not.

## Summarize

1. Because JavaScript is statically scoped, the variables needed in its internal environment are determined at compile time and do not change at runtime.

2. And because functions are first-class citizens in JavaScript, and can be called, passed as arguments, assigned to variables, or returned as functions, their runtime environment can easily change.

3. When a function returns as the return value of another function (the outer function), the variables in its outer function have been popped from the call stack, but we must make the variables it needs accessible to the inner function, thus creating a contradiction between the runtime environment and the scope at definition time.

4. So we take the variables we need in our internal environment and package them to the inner function (the closure function), which can access them at any time, creating a closed package.