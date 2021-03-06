# ES2015 Exercises

Convert the following es5 code blocks into es2015 code:

```js
var person = {
    fullName: "Harry Potter",
    sayHi: function(){
        setTimeout(function(){
            console.log("Your name is " + this.fullName)
        }.bind(this),1000)
    }
}
```

```js
var name = "Josie"
console.log("When " + name + " comes home, so good")
```

```js
var DO_NOT_CHANGE = 42;
DO_NOT_CHANGE = 50; // stop me from doing this!
```

```js
var arr = [1,2]
var temp = arr[0]
arr[0] = arr[1]
arr[1] = temp
```

```js
function double(arr){
    return arr.map(function(val){
        return val*2
    });
}
```

```js
var obj = {
    numbers: {
        a: 1,
        b: 2
    } 
}

var a = obj.numbers.a;
var b = obj.numbers.b;
```

```js
function add(a,b){
    if(a === 0) a = 0
    else {
        a = a || 10    
    }
    if(b === 0) b = 0
    else {
        b = b || 10    
    }
    return a+b
}
```

Research the following functions - what do they do?

- `Array.from`
- `Object.assign`
- `Array.includes`
- `String.startsWith`

# Next

When you're ready, move on to [Asynchronous JavaScript Review](./07-async.md)