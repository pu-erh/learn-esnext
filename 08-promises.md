# Promises

# Objectives:

By the end of this chapter, you should be able to:

- Define what promises are and how they are constructed
- Use promises to refactor asynchronous code
- Compare and contrast callbacks and promises

# Promises

Promises are almost always preferred over callbacks when managing asynchronous code. Promises make use of callback functions, but help avoid deep nesting of callbacks and improve readability. Another advantage of promises is that they are immutable, so once a promise is done, you can not accidentally call it again (something you can do accidentally with callbacks).

ES2015 introduces a native Promise API, but promises have been in use for quite a while. Different libraries have their own implementation of Promises (previously called "deferreds" in jQuery), but the idea is very similar between all of them.

Think of a promise as a **one-time guarantee of future value**. Here's a simple example:

- You go to a cafe to buy a sandwich.
- When you pay for the food, you are given a "ticket" so that other people can order while you wait. This "ticket" is the idea of a promise (a guarantee of future value).
- You keep waiting and either:
  1. Your ticket is called, you get your sandwich and you put the ticket in the trash (the promise is resolved and no one else can use the ticket)
  2. Your ticket is called, but something went wrong with the order so you return your ticket (the promise is rejected)

Let's see what this looks like. Using the native Promise API, we can create a Promise using the `Promise` constructor. This constructor takes a single function as its argument, which represents the code that should be run. This code should also specify whether the promise should be resolved or rejected, by passing in `resolve` and `reject` functions as parameters.

This syntax definitely takes some getting used to, since the `Promise` constructor accepts a function which itself accepts two callback functions! But once you've explored a few examples, it will make more sense.

Here's a function that generates and returns a new promise:

```js
function firstPromise() {
  return new Promise(function(resolve, reject) {
    var x = Math.random();
    if (x > 0.5) {
      resolve(`Hooray! Your promise was resolved with value ${x}.`);
    } else {
      reject(`Oh no, your promise was rejected with value ${x}`);
    }
  });
}
```

To understand what this code is doing, let's invoke `firstPromise` multiple times. Because `firstPromise` returns a promise, we need to tell JavaScript what to do when this promise resolves. The way to do this is with a `.then` method, which will wait for the future value that you are guaranteed to receive. `.then` accepts a callback which will run when you have that value.

In the event that the promise is rejected instead of resolving successfully, you can catch that rejection with the .catch method.

Because promises are guaranteed to either resolve or reject eventually, you can be confident that either the callback to your `.then` method will run, or the callback to your `.catch` method will run.

Enough words, let's just run the code several times and take a look at what we get back:

```js
firstPromise()
  .then(function(data) {
    return data;
  })
  .catch(function(error) {
    return error;
  });
```

If you run this code several times, you'll see that different callbacks run based on whether the promise is resolved or rejected. (Note that in Chrome console, you'll see the word "resolved" used even if it's the reject callback that runs.)

# Promises and asynchronous code

In our example above, the promise returned by `firstPromise` resolves or rejects immediately. But in most situations, this won't happen. Instead, you'll often use promises when dealing with some sort of asynchronous operation, like making an AJAX request.

We can mimic this behavior using a `setTimeout`. Let's see what this looks like, so that we can examine another example of promises.

```js
function secondPromise() {
  return new Promise(function(resolve, reject) {
    var timeToResolve = Math.random() * 5000;
    var maxTime = 3000;
    if (timeToResolve < maxTime) {
      setTimeout(function() {
        resolve(
          `Hooray! I completed your request after ${timeToResolve} milliseconds.`
        );
      }, timeToResolve);
    } else {
      setTimeout(function() {
        reject(
          `Sorry, this is taking too long. Stopping after ${maxTime} milliseconds. Please try again.`
        );
      }, maxTime);
    }
  });
}
```

There's a lot to unpack here, so let's take it line by line. First, we're randomly generating a time between 0 and 5 seconds, and storing it in a variable called `timeToResolve`. This is basically mimicking what happens when we make an AJAX request: we have no idea how long it will take for that request to complete.

After that, we're setting a maximum time that we're willing to wait, and setting that equal to three seconds.

Next comes the part where we decide whether to resolve or reject. If `timeToResolve` is less than 3 seconds, then we'll resolve the promise after `timeToResolve` milliseconds. But if it's greater than 3 seconds, we'll reject the promise after waiting for 3 seconds.

Like before, the best way to understand this code is by invoking the function several times and waiting for it to resolve or reject:

```js
secondPromise()
  .then(function(data) {
    console.log(data);
  })
  .catch(function(error) {
    console.log(error);
  });
```

If you run this code, you'll sometimes see that the promise resolves after a random time that's less than 3 seconds. Other times, you'll wait 3 seconds and then the promise will be rejected.

# Promises in jQuery

As of version 3, jQuery has built-in support for promises! This means that when you use methods like `$.get`,` $.getJSON`, or `$.ajax`, you can chain `.then` and `.catch` methods to the end of them.

Let's revisit some of the examples we looked at in the previous section.

```js
function getJokesAbout(term) {
  return $.getJSON(`https://icanhazdadjoke.com/search?term=${term}`);
}

getJokesAbout("spider")
  .then(function(data) {
    console.log("Here is our joke data!", data);
  })
  .catch(function(err) {
    console.log("Oops, something went wrong", err);
  });
```

# Promise.all

The other nice thing about Promises is that you can wait for multiple promises to resolve with the `Promise.all` function. `Promise.all` accepts an array of promises, and returns a new promise to you that will resolve only after each promise in the array has resolved. If any one of the promises in the array passed to Promoise.all gets rejected, the promise returned by `Promise.all` will be rejected too.

Let's take a look at an example:

```js
function getJokesAbout(term) {
  return $.getJSON(`https://icanhazdadjoke.com/search?term=${term}`);
}

Promise.all([
  getJokesAbout("spider"),
  getJokesAbout("ghost"),
  getJokesAbout("pizza")
])
  .then(function(data) {
    console.log("Woah check out all this data", data);
  })
  .catch(function(err) {
    console.log("Oops, something went wrong!");
  });
```

Note that this has one clear benefit over the callback pattern we saw before - namely, we only need one callback to catch errors, rather than three!

`Promise.all` is helpful if you want to fire off a lot of requests that don't have anything to do with one another. But sometimes you'll want to chain promises together sequentially. This might happen, for instance, if you need the response from one resolved promise in order to create another promise.

Fortunately, we can also return the value of promise to another. This is another way for us to escape out of callback hell. Rather than deeply nesting callbacks, we can chain together multiple promises using `.then` instead! Recall our callback example from the previous section:

```js
$.getJSON(
  "https://icanhazdadjoke.com/search?term=spider",
  function(data) {
    console.log("Here's some data on spider jokes: ", data);
    $.getJSON(
      "https://icanhazdadjoke.com/search?term=ghost",
      function(data) {
        console.log("Here's some data on ghost jokes: ", data);
        $.getJSON(
          "https://icanhazdadjoke.com/search?term=pizza",
          function(data) {
            console.log("Here's some data on pizza jokes: ", data);
          },
          function(error) {
            console.log("Oops something went wrong!", error);
          }
        );
      },
      function(error) {
        console.log("Oops something went wrong!", error);
      }
    );
  },
  function(error) {
    console.log("Oops something went wrong!", error);
  }
);
```

Here's how we can refactor this code the to use the chainable nature of promises:

```js
$.getJSON("https://icanhazdadjoke.com/search?term=spider")
  .then(function(data) {
    console.log("Here's some data on spider jokes: ", data);
    return $.getJSON("https://icanhazdadjoke.com/search?term=ghost");
  })
  .then(function(data) {
    console.log("Here's some data on ghost jokes: ", data);
    return $.getJSON("https://icanhazdadjoke.com/search?term=pizza");
  })
  .then(function(data) {
    console.log("Here's some data on pizza jokes: ", data);
  })
  .catch(function(error) {
    console.log("Oops something went wrong!", error);
  });
```

Not only have we eliminated the nesting, we've also reduced code duplication since we can catch all potential errors with a sincle `.catch`!

Let's look at one final example before moving on. Here's another example that returns values from one promise to another promise:

```js
function first() {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log("first is done");
      resolve(10);
    }, 500);
  });
}

function second(previousPromiseData) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log("second is done and we just got", previousPromiseData);
      resolve(previousPromiseData + 10);
    }, 500);
  });
}

function third(previousPromiseData) {
  return new Promise(function(resolve, reject) {
    setTimeout(function() {
      console.log("third is done and the total is", previousPromiseData);
      resolve();
    }, 500);
  });
}

first()
  .then(second)
  .then(third);
```
