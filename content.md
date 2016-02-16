## BUILD TOOLS AND GULP
------
*Premise*: In this class, we will discuss how we can use NodeJS to make writing javascript easier to manage; we will talk about how we can automate “installing” dependencies such as jQuery and underscore using NPM and how the CommonJS spec

### Topic Breakdown

#### Installing NodeJS
* we will begin by installing NodeJS
* we will write a few small program ins in node such as a simple “Hello Wrold!” program

#### NPM and setting up Node Projects
* we will discuss how to set up a “node project” 
* we will discuss NPM, node’s package manager
* we will write a few small programs in Node to understand what the equivalent of `<script src=’/js/..’></script>` is in NodeJS

#### Using Node to make frontend JavaScript easier
* we will discuss some tools that exist in Node that make it easier to write frontend javascript
* we will talk about browserify, which allows us to pull in jQuery and other libraries without using a script tag
* we will talk about how we can “compile” our javascript and why we need to “compile” javascript in the first place
* we will create a small API based reddit search using the new concepts we learned

## INSTALLING NODE.JS
------

### WHAT IS NODE

Traditionally, the only place we could write and run our javascript programs was in browsers like Chrome, Firefox, and Safari. This severely limited what we could do with the javascript language since it that could only run in a program (the browser) and not directly from our computer.

NodeJS is a project that takes all the parts of the browser code that reads, understands, and runs our javascript code and makes it work directly from our computer. In other words, Node.js allows us to run javascript that has access to stuff like the computer’s file system. This is super important because this move allows javascript to do all sorts of interesting stuff like:

1. read files on your computer
2. write new files on your computer
3. access a database
4. update a database
5. run scripts over and over again on a certain time interval

Points 1 and 2 will be super useful to us later on because we can use those abilities to run stuff like Gulp.js. Points 3 and 4 allow us to effectively write “backends” in javascript. Point 5 allows us to write all kinds of useful scripts and have them run say every night at midnight. (One example would be: a script that hits the facebook API at midnight every day and adds up all the likes you’ve given for that day).

### INSTALLING NODE

The best way would be directly from nodejs.org. Choose **v5.6.0**

### WRITING PROGRAMS IN NODE

Writing javascript code in NodeJS is a bit different from writing javascript code in the browser.

Here's a quick breakdown of how you should set up a node program:

##### Step 1:
Create a script.js file (you can call it anything you like, just needs to end in .js)

##### Step 2:
Write javascript syntax as you normally would, but now inside this script.js

**IMPORTANT NOTE**: because NodeJS runs in your browser, certain frontend javascript concepts such as the `window` object or the DOM API don't exist (the javascript is not running in a webbrowser frame, so there is no window)

##### Step 3:
Once you are ready to test your code, open up a terminal window and navigate to the folder your script.js lives in. Then, run `node script.js` to run the lines your wrote in your program.

### EXERCISES for Installing Node

**Write your first NodeJS Program.**

Requirements: 
1. Create a script called hello.js
2. Your program should print out "Hello, Wrold" when run from the terminal

**For Looping in NodeJS**

Requirements:

1. Create a script called count.js
2. Your program should print out the numbers 1 to 100 when run from the terminal

## NPM and setting up Node Projects
------

### LOADING LIBRARIES IN NODEJS

One important aspect of javascript programming that we specifically glazed over in the last section was the question of loading dependencies. In frontend programming,  we load in dependencies with `<script>` tags. For example, to load jQuery, we would do something like:

```html
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
```
In node, however, we do something like this...

```javascript
var $ = require('jquery');
```

### NPM: THE PACKAGE MANAGER FOR NODEJS

NPM stands for Node Package Manager (this comes built in to your NodeJS installation by default). Most other programming languages already have robust package management, when javascript finally moved off of the browser environment, NPM was created to be the javascript equivalent of pip (python's package manager) or composer (PHP's package manager).

Package managers are how dependencies are loaded in traditional programming languages -- this reduces the need for including long form paths and script tags. Also, with a good package management system, the order in which we load in our dependencies no longer matter. 

The way NPM works is this: 
* all packages that a node program needs lives in the same folder as that program
* you can load in packages that you create (we can call these "custom dependencies" _or_ you can load in packages from the NPM registry (an online repo of all the published packages people have written).
* everything your program needs to run lives inside the same folder, so transferring your work to a different computer is very simple

### SETTING UP A NODE PROJECT WITH NPM

All non-trivial node programs should be set up this way.

##### Step 1: 

Create a folder that will hold ALL the code and dependencies that your NodeJS program will need. Navigate to that directory in your terminal.

##### Step 2:
In your terminal, type in this:
```bash
$ npm init
```

What this does is kick start all the folders and other stuff NPM needs to initialize dependencies for your projects.

##### Step 3:

Follow the prompts that NPM asks you after running the init method. You can just press enter if you want the default values to go through.

##### Step 4:

List out your files in terminal. You can do this by running the following:

```bash
$ ls -ahl
```

You should see a new file has been created, called **package.json**. This file is extremely important. It probably looks something like this: 

```json
{
  "name": "YOUR_PROJECT_NAME",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

This file becomes more important if you are attempting to publish your NodeJS program to the NPM registry as a module other people can use. For the time being, the important things to note is that the `"main"` key should point to whatever file that contains your javascript program. 

### INSTALLING A DEPENDENCY FOR YOUR PROJECT

Now that you have a shiny new NPM enabled project folder, it's time to start installing dependencies and making stuff do things!

To begin, let's write a simple program that looks up a webpage and prints out the HTML source of that page in the console.

Looking up a webpage is something that we _could_ write on our own, but it would be much faster if we just used one of the many HTTP request libraries that exist on the NPM registry. 

I really like the [**superagent**](https://www.npmjs.com/package/superagent) library.

Let's get started.

##### Step 1:

Let's first install this package to our Node project.

Make sure you navigate to your Node project first!!

```bash
$ npm install --save superagent
```

What this does is look up the package (or module, whatever you want to call it) called "superagent" in the NPM registry and download it to your project folder. 

The `--save` flag is super important, what this does is **update your package.json automatically** and lists this package as a dependency to your project. If you go to sublime and look at your package.json now, it should look something like this:

```json
{
  "name": "YOUR_PROJECT_NAME",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "superagent": "^SOME_VERSION_NUMBER",
  }
}
```

You should also notice something else -- **there is a new folder in your directory**: the **node_modules** folder has been created.

If you take a quick peek inside this folder, you'll see a folder called **superagent**. All the files needed for the superagent dependency that was downloaded from the NPM registry was added here automatically. If you add additional dependencies from the NPM registry, they will be installed automatically in the **node_modules** folder. 

** THE IMPORTANCE OF PACKAGE.JSON **

Your `package.json` file describes all the dependencies your project needs to run. This is super important: if you need to move your project to a different computer (say a coworker wants to join the project or you want to deploy your project to a production server), all you have to do is run 
```bash
$ npm install
```
If NPM finds a `package.json` in your project folder, it will install all the dependencies listed in the package.json automatically.

**PROTIP**

When you git commit stuff, do _NOT_ commit your node_modules folder -- but _DO_ commit your package.json. The `node_modules` folder can get very large in size which makes uploading / committing stuff annoying. 

But, since your package.json has all the dependencies you need, all you have to do is run `npm install` if you need to add additional dependencies on different machines running your code.

To ignore node_modules files from git, do this:

Open the `.gitignore` file on a git enabled project. Then, type: 

```
node_modules/*
```

And save and commit.

##### Step 2:

Whew, now that we have that out of the way, let's do some cool stuff with superagent!

Since in your package.json, you pointed the `"main"` property to index.js, create an index.js file and type the following:

```javascript
var request = require('superagent');
```

This is the equivalent of you including say jquery to a frontend javascript environment. Let's take a moment and dissect this line.


**`require`**: this is a special keyword and only works in NodeJS.  What require does is automatically look up and load the package you need for your particular program to work. 

**`require('superagent')`**: as is clear from here, `require` is a function that takes in one argument -- this argument is a file path that Node looks for in your project directory. One super important thing to keep in mind: **for packages that you download from the NPM registry _in this folder_, you just have to put the name of the package** and node will know what to do. 

If you want to require a package that you wrote yourself or include an additional javascript file, all you have to do is: `require('./myAwesomePlugin')`; where `"myAwesomePlugin.js"` is the filename of a script that lives on the same level as the file you are calling that require in.

**`var request = require('superagent');`** basically what this does is load the superagent package from the **node_modules** folder. All dependency files in NodeJS can "return" one thing, this is exactly like how a function can "return" one thing. The `var request` is saving whatever the `require("superagent")` is returning so that you can use it in your index.js.

##### Step 3:

Just for kicks, let's explore what is inside the `request` variable we saved. In your `index.js` file, type the following:

```javascript
var request = require('superagent');

console.log( typeof request, request );
```

To run this code, simply go back to terminal, navigate to your NodeJS project folder and run:

```bash
$ node index.js
```

Note how the `tyepof` the `request` variable is a function. Basically, this is **exactly** equivalent to how jQuery works. In fact, we can test out the equivalency pretty easily: jQuery is on NPM! (Though for our purposes right now, it's actually pretty useless).

To install jQuery, run:

```bash
npm install --save jquery
```
Then, in your `index.js` type the following:

```javascript
var request = require('superagent');
var $ = require('jquery');

// REQUEST VARIABLE LOGS
console.log( 'TYPEOF request: ', typeof request);
console.log('===============================');
console.log( 'REQUEST VARIABLE: ', request );
console.log('===============================');

// $ VARIABLE LOGS
console.log( 'TYPEOF $: ', typeof $);
console.log('===============================');
console.log( '$ VARIABLE: ', $ );
console.log('===============================');
```

To run this code, simply go back in terminal, navigate to your NodeJS project folder and run:

```bash
$ node index.js
```

You should see something similar to this:

```html
TYPEOF request:  function
===============================
REQUEST VARIABLE:  function request(method, url) {
  // callback
  if ('function' == typeof url) {
    return new Request('GET', method).end(url);
  }

  // url first
  if (1 == arguments.length) {
    return new Request('GET', method);
  }

  return new Request(method, url);
}
===============================
TYPEOF $:  function
===============================
$ VARIABLE:  function ( w ) {
                if ( !w.document ) {
                    throw new Error( "jQuery requires a window with a document" );
                }
                return factory( w );
            }
===============================
```

Although this code looks daunting and difficult to read, the **important** takeaway here is that both the `request` variable and the `$` variable are both **functions** that return objects. 

This means we can use the `superagent` library in the same way we use jQuery:

```javascript
// in jQuery we do something like:
var myDiv = $('div');
// then we can call methods on the myDiv variable:
myDiv.addClass('foo');
```
(NOTE: if you ran the code above, you would not get expected results since jQuery does not run properly in NodeJS -- it's meant to work in browsers only).

With that being said, both libraries are _javascript_ libraries that are basically functions so in `superagent`, we can do something like:

```javascript
// in superagent we do something like:
var getRequest = request('http://www.nytimes.com');
// then we can call methods on the getRequest variable
getRequest.end(function(err, res) {
    console.log( res.text );
});
```

The result of the code block above would be a dump of all the HTML code that gets loaded when you type in `http://www.nytimes.com` into the web browser.

And that's it! We have now successfully created a javascript program that leverages a dependency downloaded from the NPM registry.

### USING A CUSTOM MADE DEPENDENCY FOR YOUR PROJECT

What's really great about NodeJS is that we can also `require` scripts that we wrote (and not just packages we downloaded from the NPM registry). 

To create a custom package that you can include in your `index.js` program, do the following.

##### Step 1:

Create a new file in the same folder as your index.js. You can call it anything you want -- let's call ours `config.js`

##### Step 2:

In our config file, let's add the following:

```javascript
var config = {
    urlToGET: 'http://www.nytimes.com'
};
```

What this config.js file does for is is create an object where we can store some aspects of our program that we don't want to intermingle with our index.js. 

Today we are doing a get request for NYTimes, but maybe in the future we'll want to change that request to a different website. If we keep all the "specifics" of our app in a different file, we can easily switch them out without having to go inside our index.js that actually runs the important stuff (and this ensures we lower the risk of us making a typo or breaking something in that code).

##### Step 3:

On the bottom of the config file, add this line:

```javascript
var config = {
    urlToGET: 'http://www.nytimes.com'
};

module.exports = config;
```

The `module.exports` line is important -- this line is the equivalent to a `return` in a normal function. Because we set our `module.exports` to be the `config` object, when we include this file via the `require` statement in any other file, we will be able to access the properties of this object.

So, in `index.js`, we could do:

```javascript
// NOTE the './' we put before config -- this tells Node that
// the config.js file is in the same level as index.js
var config = require('./config'); // the config variable now holds
//  the object from above

// in superagent we do something like:
var getRequest = request( config.urlToGET ); // note we are accessing 
// the url from the config object

// then we can call methods on the getRequest variable
getRequest.end(function(err, res) {
    console.log( res.text );
});
```

Now, the index.js program is entirely based on the config js properties.

###  EXERCISES for NPM and Node Projects

** Read files in NodeJS **

Requirements:

Create a NodeJS program that includes two files: 
1. config.js
2. index.js

In your `config.js`, have a property called `fileToRead` that takes a file path. In `index.js`, use Node's built in `fs` module (you can just require it as you would an NPM registry module) to print out the contents of the `fileToRead` file to terminal.

** Write a file in NodeJS **

Requirements:

Create a NodeJS program that includes two files:

1. config.js
2. index.js

In your `config.js`, have the following properties:

1. `fileName`
2. `fileContent`

In your `index.js` file, use Node's built in `fs` module to write a file called whatever is set in `config.fileName` and set the content of the file to equal `config.fileContent`

** Create a simple copy and paste script in NodeJS **

Requirements;

This will be a fun one: write a NodeJS script that lets you copy a file from one location into another location. This problem is challenging but very rewarding!

1. Create a new NodeJS project and `npm install` the [`commander`](https://www.npmjs.com/package/commander) library
2. NOTE: it's in your best interest to copy and paste the example(s) they have on the NPM page to see for yourself how the library works
3. Write a program that expects two command line inputs:
    * a file path (to an existing file) to copy
    * a filename to copy to
4. Your program should get angry if the user does not specify a file path to copy from. It should use Node's `fs` module to read the content of the file to copy from and write a new file with that content, thus _copying_ the file.

When you run your program, let's call it `copy.js`, it should look something like:

```bash
$ node copy.js -copyfrom awesomesauce.txt -copyto superawesome.txt
``` 

** Create a command line based movie search client **

Requirements:

This one will also be fun. Take a look at the [OMDB API](http://www.omdbapi.com/). This API allows you to search for movies by doing a simple AJAX GET request. In this exercise, we will explore how we can perform a similar request using the `superagent` NPM module from earlier but combine it with the `commander` module to make it easy to search for stuff from the command line.

1. Create a new NodeJS project and install `superagent` and `commander`.
2. In your `index.js` (or whatever file your `"main"` key in`package.json` points to), set up commander to process the `search` endpoint of the OMDB API.

Here is what a sample search from OMDB API looks like:
```html
http://www.omdbapi.com/?t=the+godfather&y=&plot=short&r=json
```

Your program should expect inputs like this:

```bash
$ node search.js -t the godfather -y 1977 -plot short -r json
```

From that input, it should assemble the URL above and then use `superagent` to perform a GET request. The only input your program should need is the `-t` argument -- everything else can be blank if needed.

The output should be formatted like this:
```bash
$ Search results:
=================
Title: The Godfather
Year: 1992
Rated: R
Plot: The aging patriarch of an organized crime dynasty transfers control of his clandestine empire to his reluctant son.
=================
```

The above should be repeated if more than one result is found.

## Using Node for frontend JavaScript
------

Let's kick things off by discussing **why** we'd need NodeJS to run frontend javascript. And then we'll dive into **how** we can actually pull this off.

### WHY NODE IS NEEDED ON THE FRONTEND

The method of using `require` to pull in javascript dependencies is called the _Common JS_ specification of javascript. It's a scheme for making dependency management in javascript easier. If you've ever worked on a large frontend project with multiple dependencies and javascript files, you'll know what a nightmare it is to load in js files with multiple `<script>` tags. 

However, the Common JS method is refreshingly simple. We no longer have to worry about `<script>` tags or the order they are placed in -- we just `require` them and call it a day. Plus, many plugins and newer javascript files are increasingly being published on the NPM registry, which makes is super easy to install and use dependencies.

...the only problem is this: **the `require` construct is not supported in browsers!**

And this leads us to **how** we can use NodeJS to run frontend javascript.

###  HOW NODEJS CAN BE LEVERAGED TO USE REQUIRE() IN FRONTEND JS

Even though `require` is not supported in the browser, _it is_ supported in NodeJS. 

**Browserify** is a npm module that takes your NodeJS programs (with all of its `require` calls) and creates a **single** js file that you can include as a `<script>` tag in your HTML files. Basically, it takes something like this:

** index.js **
```javascript
var config = require('./config');

console.log( config.foo );
```

** config.js **
```javascript
var config = {
    foo: 'Hello, Wrold!'
};

module.exports = config;
```

and builds a file that looks like this:

** bundled.js **
```javascript
(function e(t,n,r){function s(o,u){if(!n[o]){if(!t[o]){var a=typeof require=="function"&&require;if(!u&&a)return a(o,!0);if(i)return i(o,!0);var f=new Error("Cannot find module '"+o+"'");throw f.code="MODULE_NOT_FOUND",f}var l=n[o]={exports:{}};t[o][0].call(l.exports,function(e){var n=t[o][1][e];return s(n?n:e)},l,l.exports,e,t,n,r)}return n[o].exports}var i=typeof require=="function"&&require;for(var o=0;o<r.length;o++)s(r[o]);return s})({1:[function(require,module,exports){
var config = {
    foo: 'Hello, Wrold'
};

module.exports = config;

},{}],2:[function(require,module,exports){
var config = require('./config');

console.log( config.foo );

},{"./config":1}]},{},[2]);
```
Even though this code looks crazy, the important thing to note here is that in this _bundled.js_ file, we see that the config code comes before our `console.log` code since the `index.js` file _depends_ on the `config.js` file.

**In other words, browserify will resolve all of your dependencies and generate a file with the content of all the files in order of use**. Although `require` is not defined in the browser by default, the file that browserify generates **is safe to use in the browser**.

You run browserify like so:

```bash
./node_modules/.bin/browserify -e index.js -o bundled.js
```

Where:

** -e, --entry **: the main "entry" file of your program, this is basically the same as what you have listed as the **"main"** keyword in your `package.json`

**-o, --output**: the output file where your concatenated code will be written to.

Note how browserify lives in the node_modules folder -- it's just another javascript file! The way it works is it opens up your index.js files (using the same technique you used earlier to read the file contents). Then, it converts your code into what's called an _Abstract Syntax Tree_, which allows it to pull out all the `require` statements. It then opens up all the files from the require statements and performs the AST operation on those files as well. When it's done, it builds (ie: writes, using the same technqiue you used to write a file earlier) a single file called whatever you state in the _--output_ parameter -- in the order of your `require` statements in your `index.js` file.

###  AUTOMATING YOUR BROWSERIFY BUILDS 

As great as browserify is, re-running a node script for **every**. **single**. **change**. you make is super annoying and inefficient.

In order to rectify this, we will use another __awesome__ NodeJS project called `gulp`. Gulp is a taskrunner that allows you to create `tasks` that are common things you do with your code (not programming things, but more like code organization things such as running browserify).

We will see that there are a lot of benefits of having a gulp taskrunner set up for your NodeJS project, but for starters we will explore how we can automate the browserify script with gulp.

###  SETTING UP A SIMPLE GULP FILE 

Before we delve into _how_ gulp works, let's set up a simple example and see it in action.

##### Step 1:

You'll need to install the proper NPM packages to make gulp work. Gulp itself is an NPM module, but the way we will install it is _slightly_ different from how we usually install stuff:

```bash
$ npm install -g gulp
```

Note the __`-g`__ flag in place of the __`--save`__ flag. What this does is install gulp as a __global__ NPM package that you can use from __anywhere__ in your terminal. Because gulp is a tool you will probably be using a lot, it makes sense to include gulp globally. Once you do this, you can run gulp simply by doing this:

```bash
$ gulp
```
 instead of doing this:

```bash
$ ./node_modules/.bin/gulp
```

(which, note, is how we ran __browserify__ earlier).

##### Step 2:

You will also need to install another NPM module:

```bash
$ npm install --save vinyl-source-stream
```
We will discuss what this module does and why it is needed later on. For now, let's move on and start coding!

##### Step 3:

Now that we have the proper files (and assuming you already have browserify installed on this folder), let's create a file in your Node JS project folder called __`gulpfile.js`__. It's super important that this file is called gulpfile.js -- gulp will look for this file specifically when you run it.

Inside your __`gulpfile.js`__, add the following:

```javascript
// require the necessary dependencies
var gulp = require('gulp');
var source = require('vinyl-source-stream');
var browserify = require('browserify'); // yes, browserify can 
// also be called inside a .js file in Node

// set up tasks
gulp.task('browserify', function() {
    // this task will run browserify, just like we did from terminal
    // but inside this js file instead
    return browserify('./index.js')
        // this .bundle() method walks through the code
        // and concatenates all the dependencies in order
        .bundle()
        // here, we take the concatenated dependencies
        // and "pipe" it to the bundled.js file
        .pipe( source( 'bundled.js' ) )
        // then we write this file to the current directory
        .pipe( gulp.dest('./') );
});

// this is the awesome part
// with "watch", we can tell gulp to "watch" all the files
// in this directory and IF any of those files changes
// we instruct it to call our browserify task
gulp.watch('*', ['browserify']);
```
##### Step 4:

Finally, let's take this bad boy out for a test drive. In terminal, type in 

```bash
$ gulp browserify
```

You should see something like:

```bash
$ gulp browserify
[11:53:19] Using gulpfile ~/[PATH_TO_YOUR_NODE_PROJECT]/gulpfile.js
[11:53:19] Starting 'browserify'...
[11:53:19] Finished 'browserify' after 101 ms
[11:53:19] Starting 'browserify'...
[11:53:19] Finished 'browserify' after 28 ms
```
And you're good to go! Keep this terminal tab open -- it will log your gulp changes as you update your javascript files. If you want to quit watching, simply press __`Ctrl+C`__ and __then__ exit your terminal.

Now, go back to your index.js file and make a change and save -- you should notice that your terminal tab updates and looks kind of like this:

```bash
$ bash
[11:53:19] Using gulpfile ~/Desktop/foo/gulpfile.js
[11:53:19] Starting 'browserify'...
[11:53:19] Finished 'browserify' after 101 ms
[11:53:19] Starting 'browserify'...
[11:53:19] Finished 'browserify' after 28 ms


[11:53:37] Starting 'browserify'...
[11:53:37] Finished 'browserify' after 22 ms
[11:53:37] Starting 'browserify'...
[11:53:37] Finished 'browserify' after 30 ms
```
Basically, when you run gulp, it will execute the `browserify` task __once__, then begin watching the directory. Whenever it notices a file change, it will re-run the gulp task.

The main benefit here is that you can now write your frontend javascript code using `require` and not have to worry about `script` tags ever again!

###  ABOUT THAT VINYL-SOURCE-STREAM... 

Before we move on, we have to address that mysterious NPM module we installed -- `vinyl-source-stream`. 

To understand why we needed that package in the first place, we have to understand gulp's main mantra: __everything is a stream__.

A __stream__ is a special kind of javascript object that works very well with NodeJS. On the most basic level, it's an object that does a specific thing (ie: reads a file, writes a file, etc).

__BUT!__ the main difference is that instead of doing the traditional method and callback approach, ie:

```javascript
var fs = require('fs');

fs.readFile('./index.js', 'utf-8', function( err, data ) {
// data contains the full content of the file
});
```

streams work a little like this:

```javascript
var fs = require('fs');
var readableStream = fs.createReadStream('file.txt');
var data = '';
var chunk;

readableStream.on('readable', function() {
    while ((chunk=readableStream.read()) != null) {
        data += chunk;
    }
});

readableStream.on('end', function() {
    console.log(data)
});
```

Initially, this may look a little unfamiliar and weird -- it kind of is. The main advantage to using streams is that once open, they can be manipulated in various ways before closing.

For example, continuing with our previous example of the readableStream, if we wanted to write a copying program similar to what we wrote earlier, with streams we can do something like the following:

```javascript
var fs = require('fs');
var readableStream = fs.createReadStream('file.txt');
var writeableStream = fs.createWriteStream('file_copy.txt');

// pipe lets us easily take the stream from readableStream
// and push or "pipe" it into the write stream for writeableStream
readableStream
    .pipe( writeableStream, { end: false } );
// if we want to control what happens once they are both done
// we can add the .on() event we saw in previous example
readableStream
    .on('end', function() {
        console.log('copied!');
    });
```
Also note that streams are __asynchronous__ meaning if you want something to happen _after_ the stream stuff is complete, it needs to be in an 'end' event somewhere.

 
Also note how similar the code above is to our gulp task, in fact creating and piping streams is exactly what Gulp does and this is the reason why we have syntax like:

```javascript
return browserify('./index.js')
    .bundle()
    .pipe( source( 'bundled.js' ) )
    .pipe( dest( './' ) );
```

What is happening in the code snippet above is gulp is creating a new Stream which generates a string with the full browserified javascript code, then it is "piping" that stream in a file called bundled and saving it to a particular directory.

In other words, if we wanted to make other changes to our javascript code before saving it to bundled.js, all we have to do is use the __pipe()__ method.

So for example, if we wanted to minify our bundled.js code, all we would have to do is:

```bash
$ npm install --save gulp-uglify gulp-streamify
```

Then, in our gulpfile.js

```javascript
var gulp = require('gulp');
var source = require('vinyl-source-stream');
var browserify = require('browserify');
var uglify = require('gulp-uglify');
var streamify = require('gulp-streamify');

gulp.task('browserify', function() {
    return browserify('./foo.js')
        .bundle()
        .pipe( source( 'bundled.js' ) )
        // we take our browserified stream and run it through
        // uglify's minifier
        .pipe( streamify( uglify() ) )
        .pipe( gulp.dest('./') );
});

gulp.watch('*', ['browserify']);
```

Notice how we added the `gulp-uglify` package -- this is a port of the original uglify package that was built to work with gulp. However, gulp went through a few updates itself and now works purely with streams. This is where `gulp-streamify` comes in. It turns gulp plugins into Stream objects so that we can pipe them as seen in the examples above.

If we wanted to add additional functionality, like sourcemaps generation, events that handle errors and so on, we could do so simply by adding more pipes to the task above.

In the exercises that follow, we will explore a wide array of other tasks we can run with gulp. Some of these exercises we will complete as a class and others you will have to work in groups to complete. 

###  EXERCISES for using Node on frontend javascript

** Add sourcemap support to the browserify task **

Background:

We are browserifying and uglifying our code! This is great but it's hard to track down an error that happens on our browser when the js runs.

Luckily, sourcemaps can help us solve this problem. Sourcemaps basically take minified files and point them to places in the actual codebase, so we can easily track down __where__ the error is occuring.

Requirements:

1. Install the gulp-sourcemap package 
2. In the appropriate task, pipe in `sourcemaps.init({loadMaps: true})` so that your bundled.js files can point back to the proper NodeJS script where the error is happening

** Add a config.js dependency to your gulpfile **

Background:

Just like before, let's try and separate the specifics of our app from the actual implementation logic. There's literally no reason not to since we have `require` at our disposal. Plus, chances are you might want to resuse your gulpfile setup across multipe projects,  so having a config.js where you can quickly update entry points and bundle names would be super useful.

Requirements:

Create a config.js file that looks like this:

```javascript
var config = {
    dev: { // the values of the keys below can be different
        entryPoint: "index.js",
        bundleName: "bundle.js",
        bundleDest: "./"
    }
}
```

The reason why we are using this configuration is because later on, you might want to have different "build environments". For example, maybe you don't want to pipe in sourcemaps for the production build of your code -- in that case, all you have to do is create a new key in your config and write a task in your gulpfile that will ignore the sourcemap pipe.

Also, in an upcoming exercise, you will handle building and watching your CSS files with gulp as well, to make that process easy, you can just add a new key called "css" and specify entrypoints, bundleDests for that here as well.

1. Include the above config.js in your gulpfile
2. create a gulp task called "dev" or "dev-js" that bundles, uglifies, sourcemaps, and writes out your js file to wherever you specify in `config.dev.entryPoint`, `config.dev.bundleName`, `config.dev.bundleDest`.
 
** Add LESS support to gulp **

Background:

LESS is an environment that allows you to write smarter CSS. It's a superset of CSS meaning all of your regular CSS will work just fine, but you can also do some really cool stuff like save certain colors and font-sizes as variables and create "mixins", basically the CSS-equivalent of functions.

In this exercise, we will implement LESS support with gulp

Requirements:

1. Go to the [less](http://lesscss.org/) website and follow the Get Started guide
2. Install the NPM `gulp-less` package.
3. In your config.js, add a new key to the object called "less" and set the entrypoint and bundleName, bundleDest properties
4. Implement a gulp task that takes an entry point, runs less on it and builds a css bundle file. Don't forget to pipe in `sourcemaps` so you can debug!
5. Add this task to your `gulp.watch` so that your css will "recompile" everytime you make a change to your .less files


** HTML Minnify **

Background:

Just like JS and CSS can be minified, HTML can _also_ be minnified. The major usecase of this is compressed HTML can load faster in the browser.

Requirements:

1. Install the `gulp-htmlmin` plugin from NPM
2. Add a new block to your config.js that controls your HTML file entry points, bundles, etc
3. Create a new task, called 'html-min' that will minify your HTML code when the task is run. Don't forget to pipe in `sourcemaps` so you can debug!
4. Add this task to your `gulp.watch` so that your HTML recompiles whenever you make changes to it

** Notifications on task completion **

Background:

Now that we have a more fully functional task runner, it would be great if we could have a better way to tell once the tasks have completed.

We will approach this in two ways:

1. We will have a simple output in console that informs us that gulp has completed building stuff
2. For Mac users, we will also add a custom desktop notification command that will inform us no matter where we are (so we don't have to stare at the terminal waiting for task to complete)

Requirements:

1. Install NPM `gulp-util` package
2. For your js and css tasks, bind two events: the `end` and `error` events and use the `gulp-util` package pipe out the results to console

NOTE: you can just use console.log if you'd prefer, but gulp-util gives us some nice preformatting -- that's the only reason we are using it. Also, to help you guys practice reading documentation! Withholding examples on how to use this package on purpose!

For Mac Users:

Consider this:

```javascript
var exec = require('child_process').exec;

exec([
    "osascript -e",
    "'display notification",
    "\"SOME_MESSAGE_HERE\"",
    "with title",
    "\"SOME TITLE HERE\"", 
    "sound name \"Tink\"'"
].join(' '));
```

If you'd like to have the desktop notification support, simple copy and paste the lines above to your "end" and "error" callbacks and update the message and titles appropriately.

** Live Reload **

Background:

Another cool thing we can do with gulp is set up the environment so that if a webpage with our code is open in the browser, once the task is complete we can trigger a reload on the browser.

To pull this off, we'll need to include another gulp-plugin and -- you guessed it -- pipe another transformation to the stream.

Requirements:

1. Install the `gulp-livereload` package from NPM
2. In the relevant tasks, pipe in the livereaload transform once the bundling and dest() tasks have completed.

** Babel Support **

Background:

ECMA6 (or ECMA2015, whatever you want to call it) is out! Javascript 6 has some cool features! Everyone is doing it (including facebook) -- with gulp, you can too! 

Requirements:

1. Install `babelify ` on NPM
2. In your javascript gulp task, right before bundle(), add the following:

```javascript
.transform( "babelify", {presets: ["es2015"]} )
```

As you can see, you can add other transforms as you see fit. For example, if you were using React on this project, all you'd have to do is add the "react" string to that array to have react transformations on your ask (of course, you'd also need to require a few more NPM plugins that actually load React into your project...but that's a different story and a different class!)

** Shortcuts **

Background:

The best part about `gulp` tasks are that you can mix and match them! For example, I can do something like:

```javascript
gulp.task('dev', ['js-dev', 'less'])
```

And now, whenever I type in:

```bash
$ gulp dev
```

my `js-dev` and `less` tasks will automatically run. Given that, create a few gulp shortcut tasks that run tasks as you need them. One good set of shortcuts could be a `prod` and `dev` task that run tasks specific to production builds vs dev builds (for example, you can minify _only_ in production builds but leave it alone on dev builds, etc).

And finally, you can add a `default` task to gulp, which will run if you do:

```bash
$ gulp
```

without any tasks. 


