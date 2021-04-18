
---
title: The difference between arrow functions and ordinary functions
date: 2020-8-1 20:00:00
tags:  JavaScript
category: JavaScript
---
The ES6 standard adds a new function: Arrow Function, why is it called Arrow Function?

# Characteristics of the arrow function
Arrow functions are equivalent to anonymous functions and simplify the function definition. Arrow functions come in two formats, one containing only one expression, omitting even the { ... } and return are omitted. The other type can contain multiple statements, in which case you can't omit { ... } and return can be included, in which case you can't omit { ... } and return.

- Arrow functions have a simpler syntax than normal functions.
- The arrow function doesn't bind to this, it captures the this value of the context it is in as its own this value
 - The this object within a function is the object where it is at the time of definition, not the object where it is at the time of use.
 - Converting dynamic this to static this: This object in JavaScript has long been a headache, and you have to be very careful when using this in object methods. The arrow function, which "binds" this, solves this problem to a large extent.
 - The arrow function can make this pointing fixed, which is a great feature for wrapping callbacks.
 - Principle: the this pointing point is fixed not because the arrow function has an internal "this" binding mechanism, but because the function has no "this" of its own, which means that the internal "this" is the "this" of the outer block.
- The arrow function is an anonymous function, and cannot be used as a constructor, and cannot use the new command, or an error will be thrown. So the arrow function does not have new.target either.

Reason: What does the constructor do with new? Simply put, there are four steps

- JS will first become an object internally.
- And then pointing this in the function to that object.
- Then executes the statements in the constructor.
- Finally, the object instance is returned.
- The arrow function doesn't bind arguments, but instead uses rest arguments... So the arrow function doesn't have arguments.callee and arguments.caller.

The arguments object cannot be used; it does not exist within the function. If you want to use it, you can use the rest parameter instead. To get all arguments in an array-like form in an arrow function, you can use expansion operators to receive arguments, such as:

```js
const testFunc = (...args)=>{

console.log(args) //Arrays of output parameters

}
```
In function declarations before ECMAScript 6, their arguments were of the "simple parameter type". After ECMAScript 6, any parameter declaration that uses one of the default, remaining, or template parameters is no longer "non-simple parameters".
With traditional simple parameters, you simply bind the actual parameter passed when the parameter is called to the argument object; with "non-simple parameters", you bind the name to the value via an "initializer assignment".

The difference between the two binding modes is that usually when you bind an actual parameter to a parameter object, you only need to map the subscripts of the two arrays, whereas the initializer assignment needs to be indexed by name (for binding), so if there is a "renamed parameter" it can't be processed.

Calling with call(), apply() and bind() has no effect on this

Since this is already bound at the lexical level, calling a function with the call(), apply(), or bind() methods passes only one argument and has no effect on this.

The arrow function has no prototype property prototype.
You cannot simply return the object literal.

You need to wrap the object in parentheses if you want to return it directly, because the curly brackets are taken up and interpreted as a block of code.
The yield keyword cannot be used, so the arrow function cannot be used as a Generator function.
The arrow function cannot be followed by a line break after the parenthesis.
The arrow function does not have super.

# ES5 implementation of the arrow function (Babel conversion)

Before:
```js
// ES6

const  obj = {

    getArrow() {

        return () => {

            console.log(this === obj);

        };
    }
}
```
After:
```js
// ES5

var  obj = {
    getArrow:  function  getArrow() {
        var  _this = this;
        return  function () {
            console.log(_this === obj);
        };
    }
};
```
# Summarize
1. The this of an arrow function always points to this in its context, and no method can change its pointing, such as call() , bind() , apply() , etc. It can be said that it is precisely because there is no this of its own that it has most of the features described above.
2. The this of a normal function points to the object that called it.




