# Life Wisdoms

![capybara](http://www.trbimg.com/img-570bb13b/turbine/ct-presidential-campaign-capybara-huppke-20160411 "GIANT FURRY THING")

Hello, I'm a Python/Javascript developer and have decided to make a collection of technical questions in the hopes that it will help someone else (or myself) NOT get stumped during interviews anytime in the future.

This repo will be constantly updated with any technical questions that have bamboozled me at any one time

- PS: For better understanding, do check out the links in _further readings_ - the answers given to the questions are often written in shorthand and may not paint the best overall picture of the concept in question.

- PSS: The animal above is known as a [Capybara](https://en.wikipedia.org/wiki/Capybara)

### Table of Contents

- [JavaScript](#javascript)
- [Python](#python)

Further readings:

- [Grab's Guide to Frontend (Javascript)](https://github.com/grab/front-end-guide/blob/master/README.md)
- [Javascript Questions](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)


<a name="javascript"></a>
# Javascript 

<br>

## What is a Call Stack

A Call Stack is a data structure that basically records where in the program we are. If we call a function to execute, we push something onto the stack, and when we return from a function, we pop something off the stack.

<hr>
Further readings:


- [Understanding Javascript Event Loop & Call Stack](https://medium.com/@gaurav.pandvia/understanding-javascript-function-executions-tasks-event-loop-call-stack-more-part-1-5683dea1f5ec)

<hr>

## What are JWTs (JSON Web Tokens)

Json web tokens are an open standard that defines a compact and secure way for securely transmitting information between parties as a JSON object. Basically, information sent via JWTs can be verified because they are often signed using a public/private key pair. They're a great way to transmit information because you have the freedom of design when working with JSON.

When we talk about JWTs, authentication usually comes to mind. The two main ways of authentication are cookies and tokens. The biggest thing about JWTs is that it is stateless. That means that the back-end does not need to keep a record of tokens, all the data required
to check its validity as well as convey user information is contained inside of it.

<hr>
Further readings:


- [Json Web Token Crash Course](https://stormpath.com/blog/where-to-store-your-jwts-cookies-vs-html5-web-storage)
- [Intro to Json Web Tokens](https://jwt.io/introduction/)
- [Implementing Json Web Tokens in Node](https://medium.com/@siddharthac6/json-web-token-jwt-the-right-way-of-implementing-with-node-js-65b8915d550e)
- [What Happens if Your Json Web Token Gets Stolen](https://stackoverflow.com/questions/34259248/what-if-jwt-is-stolen)
- [Different Web Authentication Methods Explained](https://blog.risingstack.com/web-authentication-methods-explained/)
- [How Does Private/Public Key Encryption Work](https://security.stackexchange.com/questions/174599/how-does-public-private-key-encryption-work)
- [Session Vs Token Based Authentication](https://medium.com/@sherryhsu/session-vs-token-based-authentication-11a6c5ac45e4)
- [JWT (JSON Web Tokens) Are Better Than Session Cookies](https://dzone.com/articles/jwtjson-web-tokens-are-better-than-session-cookies)

<hr>

## Explain the concepts of prototypes

To understand prototypes, we first take a look at functions in JS.

When a function is created in JS, two objects are created.

1) The first one, is the function itself, (its an object and can have additional properties)
2) The second one, is an object that gets attached to the function, accessed by function.prototype

So **_WHY USE PROTOTYPES_** you ask?

Well when creating objects in JS using constructor functions, it doesn't make sense to store separate instances of functions that do the same thing for each object as it is a waste of memory.

Let's look at the examples below to have a better understanding of how prototypes work:

```
// Defining an object using a constructor function

function Human(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.fullName = function() {
        return this.firstName + "" + this.lastName;
    }
}

// Adding a function to .prototype

Human.prototype.greet = function() {
    console.log(`$(this.firstName) $(this.lastName)`);
}

// existing object can now use .greet function as a result of concept known as prototypical inheritance

const alex = new Human("Alex", "Kohli");
alex.greet(); "Alex Kohli"

```

<hr>
Further readings:


- [A Definitive Guide to Javascript Prototypes](https://hackernoon.com/a-definitive-guide-to-javascript-prototypes-2c263788021e)
- [Prototypes in Javascript](https://hackernoon.com/prototypes-in-javascript-5bba2990e04b)

<hr>

<br>

## When you create a web app, how would you guard against public app methods being called in the browser:

One way we can do this is by wrapping the entire script around an immediately-invoked function expression([IIFE](https://medium.com/@vvkchandra/essential-javascript-mastering-immediately-invoked-function-expressions-67791338ddc6))

Using an IIFE helps protect your code as it will not be visible in the browser debugging tools

<br>

<br>

## Tell me what you know about NodeJS:

```
Basic one liner definition => NodeJS is a Javascript runtime environment that uses a single-threaded asynchronous 
event driven non-blocking I/O model
```
<br>
A common misconception about NodeJS is that it is single-threaded. 

<br>

**_NodeJS is not single-threaded._**

<br>
What it has is a single thread that listens for connections. As a developer, this single thread is all that is exposed to you - and all of the code you write is executed on this single thread. 

When a connection is received, Node’s listening thread executes your coded event on the same listener thread. This event either does quick, non-CPU intensive work (like returning static content to a client), or long-running I/O bound operations (like reading data from a database). In the case of the former, the listener thread does in fact block for the duration of the request, but the request happens so quickly that the delay is trivial. In the case of the latter, Node uses V8 and libuv (which it is built upon) to delegate the I/O work to a thread from an underlying pool of native C++ threads. 

The single listening thread kicks off the work to an I/O worker thread with a callback that says “tell me when you’re done” and immediately returns to listening for the next connection. 

**_It is thus plain to see that Node.js is indeed multi-threaded, though this functionality is not directly exposed to the Node developer._**

**_An important note regarding Node.js is that any CPU-intensive code which you write will block the entire system and make your application scale poorly or become entirely unresponsive._** As a result, you would not want to use Node.js when you need to write an application that will do CPU-intensive work such as performing calculations or creating reports.

<hr>

**_Blocking_** - Blocking refers to operations that block further execution until that operation finishes.

Node is often referred to as a non-blocking I/O model. This means that when requests arrive at a Node server, they are serviced one at a time. However, when the code serviced needs to query the DB for example (long running I/O operation), it sends the callback to a second queue and the main thread will continue running (it doesn't wait). Now when the DB operation completes and returns, the corresponding callback pulled out of the second queue and queued in a third queue where they are pending execution. When the engine gets a chance to execute something else (like when the execution stack is emptied), it picks up a callback from the third queue and executes it.

**_The core idea is that the single listener thread SHOULD never be blocked: it should only do fast, cheap processing or delegation of requests to other threads and the serving of responses to clients._**

**_One advantage of non-blocking, asynchronous operations is that it is really fast - and you can maximize the usage of a single CPU as well as memory._**

### Why should I avoid blocking the Event Loop and the Worker Pool?
Node uses a small number of threads to handle many clients. In Node there are two types of threads: one Event Loop (aka the main loop, main thread, event thread, etc.), and a pool of k Workers in a Worker Pool (aka the threadpool).

If a thread is taking a long time to execute a callback (Event Loop) or a task (Worker), we call it "blocked". While a thread is blocked working on behalf of one client, it cannot handle requests from any other clients. This provides two motivations for blocking neither the Event Loop nor the Worker Pool:

**_Performance:_** If you regularly perform heavyweight activity on either type of thread, the throughput (requests/second) of your server will suffer.

**_Security:_** If it is possible that for certain input one of your threads might block, a malicious client could submit this "evil input", make your threads block, and keep them from working on other clients. This would be a Denial of Service attack.

<hr>
Further readings:


- [To NodeJS or not to NodeJS](https://www.davidhaney.io/to-node-js-or-not-to-node-js/)
- [Don't block the event loop](https://nodejs.org/en/docs/guides/dont-block-the-event-loop/)

<hr>

<br>

## Typescript

- Typescript is modern Javascript + types. Provides static typing to enable type checking at compile time. Can also be used to enforce strict null checks so you never get errors like below in Javascript at run-time

```cannot read property 'x' of undefined or undefined is not a function```

- **_Dynamically-typed_** languages check the types and look for type errors during runtime
- **_Statically-typed_** languages check the types and look for type errors during compile time
- In **_weakly-typed_** languages, the type of values depends on how it is used (can concatenate strings and numbers)
- In **_strongly-typed_** languages, a value has a type and that can not change
- Javascript is a weakly and dynamically typed language
- Python is a strongly and dynamically typed language

<hr>
Further readings:


- [Dynamic vs Weak typing](https://en.hexlet.io/courses/intro_to_programming/lessons/types/theory_unit)

<hr>

<br>

<a name="python"></a>
# Python

## On Tracebacks

![traceback](https://media.geeksforgeeks.org/wp-content/uploads/20191218200140/pt.jpg "GIANT ERROR OF DEATH")

Python programs crash when our code raise exceptions that an `except` statement doesn't handle. When this happens, Python displays the exception's message and a `traceback`. Also called a `stack trace`, the `traceback` shows

1) The place in your program where the exception happened
2) The trail of function calls that led up to it

<hr>
Taken from:


- [Beyond The Basic Stuff With Python](https://github.com/gaurtvin/books/blob/main/Beyond%20the%20Basic%20Stuff%20with%20Python.pdf) 
(By Al Sweigart, same guy who made ["Automate The Boring Stuff With Python"](https://automatetheboringstuff.com/))

<hr>

## Database connection pooling 

Database connection pooling is a method used to keep database connections open so they can be reused by others.

Typically, opening a database connection is an expensive operation, especially if the database is remote. You have to open up network sessions, authenticate, have authorisation checked, and so on. Pooling keeps the connections active so that, when a connection is later requested, one of the active ones is used in preference to having to create another one.

Refer to the following diagram for the next few paragraphs:
```
  +---------+
  |         |
  | Clients |
+---------+ |
|         |-+  (1)   +------+   (3)    +----------+
| Clients | ===#===> | Open | =======> | RealOpen |
|         |    |     +------+          +----------+
+---------+    |         ^
               |         | (2)
               |     /------\
               |     | Pool |
               |     \------/
           (4) |         ^
               |         | (5)
               |     +-------+   (6)   +-----------+
               #===> | Close | ======> | RealClose |
                     +-------+         +-----------+
                     
```

In it's simplest form, it's just a similar API call (1) to an open-connection API call which is similar to the "real" one. This first checks the pool for a suitable connection (2) and, if one is available, that's given to the client. Otherwise a new one is created (3).

Similarly, there's a close API call (4) which doesn't actually call the real close-connection, rather it puts the connection into the pool (5) for later use. At some point, connections in the pool may be actually closed (6).

That's a pretty simplistic explanation. Real implementations may be able to handle connections to multiple servers and multiple user accounts, they may pre-allocate some baseline of connections so some are ready immediately, and they may actually close old connections when the usage pattern quietens down.

<hr>
Taken from:


- [what-is-database-pooling](https://stackoverflow.com/questions/4041114/what-is-database-pooling)

<hr>


## Thread Safety

A piece of code is considered **_thread-safe_** if it functions correctly during simulateneous execution by multiple threads. In particular, it must satisfy the need for multiple threads to access the same data - and the need for a shared piece of data to be accessible by only one thread at any given time.

## Creating a mysql connection pool

When two people are trying to access your database and they make the request at the same time, you need to handle this case properly - ensuring that each thread is **_thread_safe_**

One of the ways to do this is by creating a **_database connection pool_**
An easy way to do this is to use an ORM (Object Relational Mapping e.g SQLAlchemy) framework 

To create a connection pool _without_ any ORM frameworks:

1) Use the `mysql.connector.pooling` module which implements pooling.
2) A pool opens a number of connections and handles thread safety when providing connections to requesters.
3) The size of a connection pool is configurable at pool creation time. It cannot be resized thereafter.

Create your own pool and name it, myPool in the arguments of connection pooling, you can also declare the `pool_size = 3` (which is the number of database connections).

Please see below for more information:

```
dbconfig = {
  "database": "test",
  "user":     "joe"
}

cnx = mysql.connector.connect(pool_name = "mypool",
                              pool_size = 3,
                              **dbconfig)
```                             
                             
Another type of connection pool code snippet with different arguments and `pool_size = 5`: 

```
MySQLConnectionPool(pool_name=None,
                    pool_size=5,
                    pool_reset_session=True,
                    **kwargs)
```
