---
title: "How to get the total numbers of Files in a Folder with Node.js"
description: "Learn how to get the total number of files in a directory using the fs module"
date: 2018-04-30T01:05:12+01:00
draft: false
---

When working with files and directories in Node.js, the built in <a href="https://nodejs.org/api/fs.html">fs</a> module provides an api for interacting with the file system. All file system have asynchronous and synchronous nature.

The <a href="https://nodejs.org/api/fs.html">fs module</a> comes with a method **readadir** that enable Node.js to read the content in a directory. 

The code below get the total number of files in a directory.

```
var fs = require('fs');

fs.readdir ( 'path/to/your/folder', (error, files) => {
    let totalFiles = files.length; // return the number of files
    console.log(totalFiles); // print the total number of files
});

```
If your Node.js package does not support arrow function, you can use the code below.

```
var fs = require('fs');

fs.readdir ( 'path/to/your/folder', function(error, files) { 
    var totalFiles = files.length; // return the number of files
    console.log(totalFiles); // print the total number of files
});
```
Please change the *`path to your folder`* to the folder you want to access.


The **readdir** is  a method in *fs* module.

**readdir** is asychronous in nature.

It allows three parameters (path[, options], callback).

The *`files`* parameter in the callback function is an array containing all files in the directory.

*.length* is a property for getting the length of an array.

