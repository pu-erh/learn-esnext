# Class Syntax.

# Objectives:

By the end of this chapter, you should be able to:

- Explain what the the `class` keyword in ES2015 does
- Compare and contrast the `class` keyword versus writing constructor functions and prototypes

# Class syntax

Although this addition to the language is not widely loved by the community, it is important to understand because modern frameworks like React and Angular 2 use it quite frequently. All that this syntax does is obfuscate constructor functions and prototype properties/methods. There is nothing new going on here; it is just a layer of abstraction. Let's recall what this looked like in ES5:

```js
function Person(firstName, lastName){
    this.firstName = firstName;
    this.lastName = lastName;
}

Person.prototype.sayHi = function(){
    return this.firstName + " " + this.lastName + " says hello!";   
}

Person.isPerson = function(person){
    return person.constructor === Person;
}
```

Here is what an ES2015 Implementation looks like:

```js
class Person {
    constructor(firstName, lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }
    sayHi(){
        return `${this.firstName} ${this.lastName} says hello!`;
    }
    static isPerson(person){
        return person.constructor === Person;
    }
}
```

The function called `constructor` MUST be named that way (since that is what is run when the `new` keyword is used). If you just try to run Person() you will get a TypeError with the message that "Class constructor Person cannot be invoked without 'new'".

If we want to add functions directly on the "class" (which is really a function), we use the word `static`.

You can read more about this new class syntax [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), and you can read more about the `static` keyword [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static).

# Next

When you're ready, move on to [ES2015 Exercises](./06-exercises.md)