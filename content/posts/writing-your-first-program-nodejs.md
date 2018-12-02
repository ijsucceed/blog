---
title: Node.Js - Your first web application
description: Learn to install Node.js, write a console program, and create a browser
  application
date: 2018-04-29 15:04:29 +0000

---
Some basic knowledge of HTML, CSS, and JavaScript will be good for you.

If you don't know what is Node.js. Node.js is framework use for writing server side application with JavaScript. This means that you can writes a complete web application with only JavaScript. With Node.js you don't need to write web applications using different web enable languages. With just a single language, you can do both the front and the back things.

While that has be said. Let's go through installing Node.js, writing a console program, and finally creating a web browser application.

## Installing Node.Js

If you don’t have Node.js on your computer, go ahead an install it now. If your operating system does not provide you with Node.js package, you can download the installer from the <a href="https://nodejs.org">official Node.js website</a>. Node.js always provide tips for the right version to download. I recommend you download the one with label _Recommeded for most users_.

Before we move to the next step, make sure Node.js has been installed correctly on your machine. To check if Node.js is functional, open your window terminal and type `node`. Here is what you should expect to see.

    $ node
    >

The node command opens up an interactive prompt (Javascript console) where you can write some JavaScript code and get quick result. Always remember that Node.js is a framework that allows you to run JavaScript code on the server.

## Console program

Let’s print a simple welcome message using a single line of JavaScript code on the interactive prompt as follows:

    > console.log("Welcome to Node.js");
    Welcome to Node.js
    undefined

Amazingly! we just wrote our first Node.js console program. With that, be rest assured that Node.js has been properly installed on your computer. Javascript console always print out a return type. This is why the _Welcome to Node.js_ is followed by _undefined_.

Before going further, I will like to talk about **NPM**. **NPM** is a pretty package manager for managing Node.js packages, it makes things seamless when writing and managing applications. Node.js installer comes together with NPM, so NPM would be automatically installed when you install Node.js on your computer. To check if NPM have been installed on your computer, type `npm -v` on a terminal window. You should see the version of NPM installed on your computer.

    $ npm -v
    ...

Interestingly, we can now create our first Node.js application to run on a browser.

## First web program

Let’s start by creating a directory ( folder ) where our program will live. I’m going to call this directory _project1_, since it is our first project.

    $ mkdir project1

Navigate to the folder you just created.

    $ cd project1

Now inside the folder create a new file, name it app.js using any text editor of your choice and enter the following codes.

    var http = require ('http');
    http.createServer( function( request, respond ) {
        respond.write('Welcome to my first project');
        respond.end();
    }).listen(1234);
    console.log('My app is running, visit http://localhost:1234');

What this code does is pretty simple. On the first line we created a variable `http` that require a built-in module _http_, which returns a JavaScript object. A Node.js module is a JavaScript library. The _http_ module is a built-in module written in Javascript which allow us to tranfer data over the hyper text transfer protocol.  Whenever we are trying to access a module, we use the `require()` function and the name of the module would be an argument `'http'`. There are other several <a href="https://nodejs.org/api/modules.html">modules</a> we can use in our application, let just keep things simple for now. The object `http` invokes a JavaScript  method `createServer(..)` that allows us to pass a callback function with two argument `request`, `respond` . The `respond.write` push our welcome message to the body and `respond.end` write to the body and terminate the response. We then set the port `listen(1234)` in order to view the result on a web browser. The last line will simply print a message on the console, pointing to where we can view the result.

Now, to run the program we just created, on your terminal type `node app`. `app` is the name of our program file _app.js_ we created earlier. You should expect the see this. Make sure you are in the directory where you save your program.

    $ node app
    My app is running, visit http://localhost:1234

Now open up a web browser and go to http://localhost:1234 to see the result.

{{< figure src="/images/node_welcome_result.png" >}}

Awesome! that’s just the basic way to create a server side web application with Node.js framework.

If you get hooked up in any area, I recommend you start from the beginning and please don’t terminate any application you open on the course of this lesson until you arrive at the final result. I don’t think anyone will have any difficulty with this article, but feel free to reply if you have any problem.

Congratulation, you have completed the first big step in building web application with Node.js.