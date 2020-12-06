# Arrow Functions

# Objectives:

By the end of this chapter, you should be able to:

- Use ES2015 arrow functions to write shorter functions
- Explain the difference between arrow functions and the `function` keyword

# Arrow functions

As an alternative to the keyword `function`, ES2015 gives us a new function syntax called `arrow functions`. These functions are denoted by the => characters. While they are very similar to the `function` keyword, arrow functions have a few key differences:

- If the arrow function is all on 1 line - an implicit `return` is added (you do not need the keyword `return`).
- If the arrow function is on more than one line, `{}` must be used (just like a regular `function`)
- If the arrow function takes 1 argument, you don't need to wrap that argument in parentheses (though for multiple arguments, you do)
- Arrow functions are **always** anonymous

Let's see some examples:

```js
// basic examples: 
var add = (a, b) => a + b;
var shout = str => str.toUpperCase();
var multilineArrowFunction = a => {
    let b = a * a;
    return b + a;
}

// callback examples:
var arr = [1,2,3,4];

// function keyword syntax
arr.map(function(val){
    return val*2;
})

// arrow function syntax
arr.map(val => val *2)
```

Another important distinction between arrow functions and `functions` defined using the function keyword has to do with the keyword this. Arrow functions lexically bind the `this` value. What does that mean? From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions):

> Until arrow functions, every new function defined its own this value (a new object in case of a constructor, undefined in strict mode function calls, the context object if the function is called as an "object method", etc.). This proved to be annoying with an object-oriented style of programming.

Ok, but what does that mean? Let's take a look at an example:

```js
var obj = {
    firstName: "Elie",
    sayHi: function(){
        setTimeout(function(){
            console.log("Hi, I am " + this.firstName);
        },1000)
    }
}
```

If you call `obj.sayHi()` in this example, you'll see that "Hi, I'm undefined" gets logged to the console one second later. This is because the callback to `setTimeout` loses the context of `this`, as it isn't being called as a method on `obj`.

When using an arrow function for this callback, however, the arrow function keeps its value from the enclosing context: in this case, it will refer to the `obj` object, and the `console.log` will work as expected:

```js
var objES2015 = {
    firstName: "Elie",
    sayHi: function(){
        setTimeout(() => {
            console.log(`Hi, I am ${this.firstName}`);
        },1000)
    }
}
```

You can read more [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

# Next

When you're ready, move on to [Default Parameters, Rest and Spread](./03-default-rest-spread.md)