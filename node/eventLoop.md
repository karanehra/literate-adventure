# The Event Loop in Node

NodeJS is single-threaded. Only one thing runs at a time. This sounds like a limitation but helps in building large apps without worrying about concurrency issues.

While writing code one should avoid thread-blocking for example doing synchronous network calls or infinite loops. In a browser each tab has it's own event loop so as to keep the tab processes seperated from each other

