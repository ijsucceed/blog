---
title: Node.js - How to display content in HTML format
description: Learn how to quickly display html content on a browser
date: 2018-04-30 09:12:52 +0000

---
I think this is my second post on Node.js.

In this post we are going to see how to display contents in HTML  on a browser with Node.js.

Let’s start by creating a directory ( folder ) where our program will live. I’m going to call this directory _project2_, since it is our second project.

    $ mkdir project2

Navigate to the folder you just created.

    $ cd project2

Now inside the folder create a new file, name it _app.js_ using any text editor of your choice and enter the following codes.

    // Displaying html content
    var http = require('http');
    
    http.createServer( function (req, res) {
        res.writeHead( 200, { 'Content-Type' : 'text/html' } );
        res.write( '<h1> Welcome to my site </h1> ');
        res.write( '<p> You sell all your stuff here </p>' );
        res.write( '<a href="https://twitter.com/ijsucceed"> Follow me </a>' );
        res.write( '<p> &copy Mysite. All right reserved. </p>' );
        res.end();
    }).listen(1234);
    
    console.log ( 'Check result here http://localhost:1234' );

You can see that we require the _http_ module, which returns an object. Just the way I explain in the first series, the _http_ module is a built-in module written in Javascript which enable Node.js to tranfer data over the hyper text transfer protocol. The object `http` invokes a JavaScript  method `createServer(...)` that allows us to pass an anonymous function with two argument `req`, `res`. `req` and `res` is just a short hand of request and respond, and always make sure the `req` comes before the `res`. The `res.writeHead(..)` send a response header request. The `200` is a status code which implies an OK request i.e our request was fufilled. The last argument `{'Content-Type':'text/html'}` is the response header object for HTML content. By sending a response header request, whatever content we push to the body will be seen as HTML.

Now on window terminal type `node app`. When you check the result in the browser http://localhost:1234  you should expect to see this.

{{< figure src="/images/node_html_result.png" >}}

Interestingly! we just learnt how to quickly display content in HTML format.

If you get hooked up in any area, I recommend you start from the beginning and please don’t terminate any application you open on the course of this lesson until you arrive at the final result. I don’t think anyone will have any difficulty with this article, but feel free to reply if you have any problem.

Congratulation, you have completed the second big step in building web application with Node.js.