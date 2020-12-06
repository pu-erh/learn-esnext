# Let, Const, and Template Strings.

# Objectives:

By the end of this chapter, you should be able to:

- Refactor code to use `let` and `const` and explain the implications of using both
- Use template strings to avoid string concatenation in your JavaScript code

# let and const
ES2015 brings us some new words to declare variables, along with a new kind of scope. So far we have only seen `global` and `function` scope, but the keyword `let` gives us access to block scope. You can think of blocks as code inside of curly braces that are not part of an object or function definition. Blocks exist in `if` statements, `for/while` loops, `switch` statements and `try/catch` blocks. Here are some examples:

```js
let instructor = "Elie";
if (instructor === "Elie") {
    let anotherInstructor = "Tim";
}
anotherInstructor; // ReferenceError! This would not be true if we had used `var` instead of `let`.

for (let i = 0; i < 5; i++){
    setTimeout(function(){
        console.log(i);
    },1000);
}
// try replacing let i = 0 by var i = 0 in the above. What happens... and why?
```

ES2015 also gives us another keyword `const` which allows us to create constants. You can think of a constant as variables that can not be reassigned.

```js
const favoriteFood = "Pad Thai";
favoriteFood = "Pad See Ew";
// Uncaught TypeError: Assignment to constant variable.
const person;
// Uncaught SyntaxError: Missing initializer in const declaration
```

Like `let`, `const` also respects block scope. This is the biggest difference between using `let` and `const` as opposed to the keyword `var`.

# Template strings

ES2015 gives us string interpolation using back ticks (``) and adding our variables in a `${}`. Here is an example:

```js
`Now we can do string interpolation like ${1+1}`;

var person = "Elie";

`My name is ${person}`;
```

String interpolation can make working with strings much more convenient, because it allows us to avoid having to do a lot of string concatenation:

```js
var firstName = "Matt";
var lastName = "Lane";
var title = "instructor";
var employer = "Rithm";

// ughhh, so much concatenation...
var greeting1 = "Hi, my name is " + firstName + " " + lastName + ", and I am an " + title + " at " + employer + "!";

// much better
var greeting2 = `Hi, my name is ${firstName} ${lastName}, and I am an ${title} at ${employer}!`;
```

# for of loop

Another nice addition to ES2015 is the `for` `of` loop which allows us to iterate through what is called an `iterable`. We will not get in depth about `iterables`, but know that these include objects, arrays and even returned values from generators.

```js
let arr = [1,2,3,4,5];

for(let val in arr) {
    console.log(val);
}

// 0
// 1
// 2
// 3
// 4

for(let val of arr) {
    console.log(val);
}

// 1
// 2
// 3
// 4
// 5
```

When dealing with arrays, note the difference between a for in loop and a for of loop. The `for in` loop will loop over the indices of an array; the `for of` loop will loop over the elements of an array:

```js
let letters = ['a', 'b', 'c'];

for (let idx in letters) {
    console.log(idx);
}
// 0
// 1
// 2

for (let char of letters) {
    console.log(char); 
}
// 'a'
// 'b'
// 'c'
```

When you're ready, move on to [Arrow Functions](./02-arrow.md)
