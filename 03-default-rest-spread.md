#  Default Parameters, Rest and Spread

# Objectives

By the end of this chapter, you should be able to:

- Explain what ES2015 default parameters are and how to use them
- Compare and contrast the rest and spread operators and how they are used

# Default parameters

Another nice ES2015 feature is the ability to add default values to parameters in our functions

```js
// OLD - causes unintended issues because 0 is falsey!

function add(a, b){
    a = a || 12
    b = b || 13
    return a + b;
}

add() // 25
add(0) // 25 - WHY IS THIS??
add(0,0) // 25 - WHAT IS HAPPENING??

// NEW
function add(a=12,b=13){
    return a+b
}

add() // 25 - CORRECT!
add(0) // 13 - CORRECT!
add(10,10) // 20
```

Default parameters are set by assigning them when the function is defined, as you can see. You can read more about default parameters [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters).

# Rest / Spread

ES2015 gives us two new operators with the same syntax. The first is the `rest` operator (think of it like the "rest" of the arguments), and is used inside of a list of function parameters in the definition of a function to indicate the "rest" of the arguments:

```js
function data(a,b,...c){
    console.log(a,b,c);
}

data(1,2,3,4,5); // 1, 2, [3,4,5]
```

Using the `rest` operator is often a useful way to avoid dealing with the `arguments` array-like object. `arguments` can be a little tricky to deal with, since it's not actually an array and therefore doesn't have access to common array methods. However, when you use the `rest` operator, what you get access to inside of your function is a bona fide array.

```js
function checkArguments() {
    return Array.isArray(arguments)
}

function checkArgumentsES2015(...args) {
    return Array.isArray(args);
}

checkArguments(1, 2, 3); // false
checkArgumentsES2015(1, 2, 3); // true
```

Here's another refactoring example that removes reference to `arguments`:

```js
function multiply() {
    let product = 1;
    for (let i = 0; i < arguments.length; i++) {
        product *= arguments[i];
    }
    return product;
}

function multiplyES2015(...nums) {
    return nums.reduce((product, num) => product * num, 1);
}

multiply(1, 2, 3, 4); // 24
multiplyES2015(1, 2, 3, 4);
```

Now let's move on to the `spread` operator. The `spread` operator sort of does the inverse of the `rest` operator: instead of converting a comma-separated list of values into an array, it spreads an array into a comma-separated list of values. Because of this, the `spread` operator is used when invoking a function, unlike the rest operator, which is used when defining a function. Here's an example:

```js
var arr = [1,2,3,4];
function addFourNumbers(a,b,c,d){
    return a + b + c + d;
}
addFourNumbers(...arr);
```

Also, even though the `arguments` object is not an array, you can still apply the spread operator to it. Here's another example:

```js
function addThree(a,b,c) {
    return a + b + c;
}

function addThreeArgs() {
    return addThree(...arguments);
}

addThree(1, 2, 3); // 6
addThreeArgs(1, 2, 3); // 6
```

You can read more about [rest](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters) here and spread [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator).

# Next

When you're ready, move on to [Object Shorthand and Destructuring](./04-object.md)