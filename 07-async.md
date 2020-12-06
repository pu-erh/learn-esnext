# Asynchronous JavaScript Review

# Objectives:

By the end of this chapter - you should be able to

- Define key terms like threads, parallelism, and concurrency
- Understand the purpose of callbacks and where they fall short

# Key Definitions + How JavaScript Works

One of the trickier things to wrap your head around in JavaScript is the idea of managing asynchronous code. Before we learn more about the different ways to manage async code, let's define some key terms to better understand what we mean when we discuss these techniques.

# Thread

A process can have many threads if multi-threading is supported. JavaScript only supports a single thread, so there can only be a single sequential flow of control within a program. So how are we able to write asynchronous code that "seems" to be doing multiple things at once? The answer is that it really is not, it just may appear so. Let's learn more about some key terms related to this.

# Concurrency

Concurrency occurs when more than one task makes progress regardless of another task. This means that if two tasks are happening, the second does not need to wait for the first to finish before it can complete execution. Concurrency can occur on one a single thread and that is how asynchronous tasks happen with JavaScript.

# Parallelism

Parallelism occurs when more than one task runs **at the same** time as another task. This means there is no taking of turns; each process moves at the same time. To achieve parallelism in JavaScript, we would have to run multiple JavaScript processes (either through multiple Node.js processes or using [Web Workers](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers) which are not fully implemented in all browsers).

# Asynchronous code in JavaScript

So how does JavaScript do it? The answer is through the event loop! You can read more about the event loop and single threaded concurrency model with JavaScript [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop), [here](http://altitudelabs.com/blog/what-is-the-javascript-event-loop/), or watch this great video [here](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

# Callbacks

When you first learn about asynchronous code, you'll typically learn to manage it using callbacks. When you make an AJAX request, for instance, you'll often want to execute some JavaScript once you've received a response to your request. In order to ensure that your code doesn't run until you've received a response, you can often place that code inside of a callback to the original request. Here's an example of what that might look like, using jQuery to make a request to the [Dad Jokes API](https://icanhazdadjoke.com/):

```js
$.getJSON(
  "https://icanhazdadjoke.com/",
  function(data) {
    console.log("Cool, here's some joke data: ", data);
  },
  function(error) {
    console.log("Oops something went wrong!", error);
  }
);
```

Similarly, when we need to manage asynchronous code in a certain order, we can nest our callbacks:

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

One of the simplest ways of managing asynchronous code is through the use of callbacks, but callbacks get quite messy when we have to start nesting functions inside of each other. This is often referred to as the "Pyramid of Doom" or "Callback Hell." Not only do we have functions nested inside of each other, but we have another function for each possible error.

It would be easier if we had more control over our asynchronous code. This is where promises can help.

# Next

When you're ready, move on to [Promises](./08-promises.md)