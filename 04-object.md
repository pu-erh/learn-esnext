# Object Shorthand and Destructuring

# Objectives:

By the end of this chapter, you should be able to:

- Use object shorthand notation to refactor code
- Understand and use destructuring for objects and arrays

# Object shorthand notation and destructuring assignment

ES2015 gives us quite a few enhancements on objects, which allow us to write more concise code with less repetition. Let's see what that looks like:

```js
var obj = {
    firstName: "Elie",
    sayHi: function(){
        return "Hello from ES5!";
    },
    sayBye() {
        return "Bye from ES2015!";
    }
}

var person = "Elie";
var es5Object = {person: person};
es5Object; // {person: "Elie"}

var es2015Object = {person};
es2015Object; // {person: "Elie"}
```

In the first example, note that in ES2015, when you're adding a method to an object, you can eliminate the colon and the `function` keyword. In other words, these two are essentially equivalent:

```js
var o1 = {
  sayYo: function() {
    console.log("Yo!");
  }
};

var o2 = {
  sayYo() {
    console.log("Yo!");
  }
}
```

The other example shows that you can pass a variable into an object instead of a key-value pair, and JavaScript will convert the variable name into the key for you.

# Destructuring assignment syntax

ES2015 also gives us access to the destructuring assignment syntax for objects and arrays. From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment):

The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays or objects into distinct variables. This can be useful if you want to assign multiple variables at once:

```js
var obj = {
    a:1,
    b:2,
    c:3
};

var {a,b,c} = obj;

a; // 1
b; // 2
c; // 3
```

In the same way that we can destructure objects, we can also destructure arrays (which are really just a special type of an object).

```js
var arr = [1,2,3,4];
var [a,b,c,d] = arr;

a; // 1
b; // 2
c; // 3
d; // 4

var [first,second] = [1,2];

first; // 1
second; // 2
```

Array destructuring also allows us to swap the values in two variables succinctly. This can be useful if, for instance, you're writing a function which randomly shuffles elements in an array by swapping pairs of elements. Here's an example:

```js
var [a, b] = [1, 2];
a; // 1
b; // 2

[a, b] = [b, a];
a; // 2
b; // 1
```

# Next

When you're ready, move on to [Class Syntax](./05-class.md)