# The Event Loop in Node

JavaScript is a single-threaded programming language with a synchronous execution model that processes one operation after another, it can only process one statement at a time. This sounds like a limitation but helps in building large apps without worrying about concurrency issues.

While writing code one should avoid thread-blocking for example doing synchronous network calls or infinite loops. Any code that takes too long to execute and return control to the event loop will block all the execution of any other JS code(even hanging the UI on the client side). 

In a browser each tab has it's own event loop so as to keep the tab processes seperated from each other.

In JS all I/O primitives are non-blocking and hence the language relies heavily on `callbacks` or `awaits`

Although **Javascript is not asynchronous**, the inclusion of the **WebAPI** together with the **Event Loop and the Queue Callback** allow it to provide a certain aspect of asicronicity so that the heavier tasks do not block the thread of execution.


## The Call Stack

The call stack is a simple LIFO queue. The event loop keeps checking the call stack for any functions that might need to run next. This call stack can been seen in error stack traces to drill down the call stack hierarchy upto the erroneous body of code.

## Defer a Call till the Stack is empty

For such a case we use `setTimeout(functionToDefer, 0)`. The working behind this is hidden in the **Message Queue**

### Message (Callback) Queue

After the timer expired the `functionToDefer` is put in a Message Queue (FIFO).This Queue also contains user-initiated events like click or keyboard events, or also DOM events like onLoad, or fetch responses before your code has the opportunity to perform their callbacks.

The event loop gives priority to the `Call Stack` to process everything as described above. Once the call stack is empty it moves on to the `Message Queue`.

`setTimeout` doesn't block the IO as they don't execute in the same thread but the live on a thread of their own(passed on to the Web API of the browser/system). This is at the core of a Non-Blocking IO.

### Job (Microtask) Queue in ES6

Similar to the `Message Queue` but this was made to execute an asynchronous callback as soon as possible rather than putting it on the end of the call stack.

**Promises that resolve before the current function ends will be executed right after the current function.**

Example: 

```javascript
const bar = () => console.log('bar')

const baz = () => console.log('baz')

const foo = () => {
  console.log('foo')
  setTimeout(bar, 0)
  new Promise((resolve, reject) =>
    resolve('should be right after baz, before bar')
  ).then(resolve => console.log(resolve))
  baz()
}
```

Outputs: 

```
foo
baz
should be right after baz, before bar
bar
```

## Promises(todo: indepth)

It is necessary that the Call Stack be empty before the event loop can pickup callbacks from the Message Queue. This causes us to never be sure when our Callback will be actually executed. This also caused the Callback Pattern to result in `Callback Hells`.

To solve this promises were born. Described as follows: 

```
We can define a Promise as an object that can resolve to produce a single value,
at some time in the future, or the reason why it could not be resolved.
```

## 


[1]([https://link](https://www.digitalocean.com/community/tutorials/understanding-the-event-loop-callbacks-promises-and-async-await-in-javascript))