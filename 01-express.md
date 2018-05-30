[Slide How the MEAN stack fits]('./index.html')

# Express

[Express website](http://expressjs.com)

Node designed for network comm, not specifically for web

So far we've been creating webservers with Node `require('http')` and then `require('connect')` at a low level

Now it's `require('express')`

Built on top of and extends connect and makes use of its middleware architecture

uses template engines, routing system, and more

[npm.im/express](http://npm.im/express)

almost 4 million downloads in just the past week

[Slide comparing connect with express hello world]()

# Hello world using Express

* Create lesson4 folder, and create index.js

1.  Run `npm init -y`
1.  Run `npm i express` you don't need `--save` anymore. Why?
1.  Double check for node_modules and package.json. Lots of deps!
1.  Create `app.js`

```js
const express = require('express');
const app = express();

app.get('/', function(req, res) {
  res.send('Hello World');
});

app.listen(3000);
console.log('example app listening on port 3000');
```

What does '/' mean in app.get?

fyi: res.send == res.end

What happens if I goto '/asdf' ?

Why is error message GET? // default http method

What are other [HTTP methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)?

[slide showing app.VERB in ppt]()

The name of the command (get, post) corresponds to the kind of request we want to make (app.get, app.post)

## Show GET vs POST by creating a form

```html
<form action="/users/123" method="GET">
    <label for="name"></label>
    <input type="text" name="name">

    <label for="food"></label>
    <input type="text" name="food">

    <button type="submit">Submit</button>
</form>
```

1.  Try submitting some data without running a server. Notice URL is a GET request. As if we typed URL in the browser
1.  If POST, show REST Client extension code example
1.  GET reads, POST creates, DELETE erases, PUT updates

So in our app.js, you could have other verbs like:

```js
app.get('/',
app.post('/',
app.put('/',
app.delete('/',
```

* Can also use above to create APIs so that JSON data is passed instead of HTML

---

You asked: where does response and request come from? magically populated

So let's start with the below, but strip it down to its essentials

Use [repl.it](https://repl.it) to code below:

```js
app.get('/', function(req, res) {
  res.send('Hello World');
});
```

So working backwards realize that req/res are being populated before my function is being called

```js
function get(path, myFunction) {
  let request = {
    a: 'hi',
    b: 2,
    c: true
  };

  myFunction(request);
}

get('/', function(req) {
  console.log(req);
});
```

And to include the prefix app.

```js
const app = {
  a: 'hi',
  get: (path, myFunction) => {
    let request = {
      a: 'hi',
      b: 2,
      c: true
    };

    myFunction(request);
  }
};

app.get('/', function(req) {
  console.log(req);
});
```

---

# Express generator to pre-build website

[npm.im/express-generator](https://www.npmjs.com/package/express-generator)

From now on we will use the express-generator to build all our apps

Install it globally `npm i -g express-generator`

Express will create an app.js, create a structure of folders, and default pages so as soon as we start the server we will have a functioning basic website

Run `express -h` to see options. There are a few different template engines which refer to 'Views'

[Slide of mvc model]()

* views refer to our template engines. But we don't serve html files anymore because it doesn't support server side variables.
* models relate to db
* dispatcher - at a given url, do this
* controller is the logic
* We will use [EJS](http://ejs.co). But can be used with a variety of HTML templating engines
* Show [jade example](http://jade-lang.com/). Show how [fussy indents are](https://naltatis.github.io/jade-syntax-docs/)
* so `express -h` by default uses pug/jade so we have to say `-e` to use ejs. For example `express myapp -e` will create a folder called myapp and inside it will have the structure of folders, some default pages but using [ejs template engine](http://expressjs.com/en/starter/generator.html)

1.  Run `express -e lesson4-part2`
1.  `cd myapp`
1.  `npm i`
1.  `nodemon` (visit localhost:3000 to see html page)

## Examine folder structure that Express built

public/ contains static files, images, client side js, css.  
routes/ this is the dispatcher. app.get('/', ...) app.post('/', ...) etc.

### views/index.ejs

* Notice it's mostly html. The <%> tags are like php.
* Where does the value of title come from? // provided in controller
* notice different kinds of tags [<% <%= <%-](http://ejs.co/)

### View routes/index.js

* Here's where the value of title comes from
* looks similar to what we did before
* the router allows us to dispatch urls
* new method called render. 1st parameter connects to equivalent filename.ejs
* 2nd parameter is just a js object. Can pass multiple values

## Pass another key/value in res.render()

```
<p><%= message %></p>
```

Refresh and error msg is "message is not defined". Fix:

```js
res.render('index', {
  title: 'Lesson 4',
  message: 'Hello world'
});
```

But where is the controllers?

## Create controllers folder

* Currently our logic is inside our routes folder, but we want to separate that into a controllers folder. More scalable, DRY principle if same code on a different route. Hard to test. So factor it out on its own controller. Each functional part of your app should have its own controller.

1.  Create folder called controllers
1.  Inside controllers/ create indexController.js

```js
/* indexController.js */
exports.homePage = (req, res) => {
  res.render('index');
};
```

```js
/* index.js */
const indexController = require('../controllers/indexController');

router.get('/', indexController.homePage);
```

* Since code/logic isn't directly tied to route anymore, we can reuse it for other routes if need be.
* Sometimes we'll have multiple middleware listed after indexController.homePage

---

---

# Assignment 1

* unveil/unhide in Bb
* Due in 2 parts. Part 1 due next Thursday. Part 2 due Thursday after. 2 weeks total. Good news, express generator will pre-build alot of it.
* Portfolio will be useful to you. Helps you get jobs. Employers want a portfolio, linkedIn and Github moreso than resume
* View sample portfolio or google [amazing developer portfolios](https://codeburst.io/10-awesome-web-developer-portfolios-d266b32e6154)
* Assignment must have 5 pages
* use express generator to build it, use ejs template engine
* use of app.get
* use of any css framework
* all of your content can be static ejs pages loaded with express routing; so no dynamic content needed, no db
* shared header/footer
* commiting to git
* deploying to a cloud service (azure or heroku)
* bonus marks: contact page, it doesn't have to work, but if you figure it out -- bonus!
* bonus: put it under your own site, employers ask for this, how to register for a domain, how to get it online etc.
* cite where external code came from, email me to check
* So for part 1 - get started, use generator and set up your 5 views, at least 3 commits to github (not within 15 minutes)
* so if I download and run it, I can view each page. no styles needed
* big idea: you'll use this to job seek
* can create videos of non-web/IoT projects too - comm skills

# How does our app know to go to these .js files in routes/

* view app.js - more complex, more deps
* Add comment // links to our controllers or route files above `var routes = require...`
* When we say res.render how does it know where to look for index? `app.set('views', path.join(__dirname, 'views'));`
* Here is where we use -e `app.set('view engine', 'ejs');`
* didn't have to specify css file located in public since `app.use(express.static(path.join(__dirname, 'public')));`
* Add comment // Attach index.js to '/' `app.use('/', routes);`
* error handling
* making file public `module.exports = app;`

## From incoming request to ejs webpage : the flow

* incoming request hits app.js (client)
* index.js (dispatcher)
* indexController.js (controller)
* index.ejs (view)

## Create route: randomNumber

Create random.ejs in views folder

```
<h1><%= title %></h1>
<p><%= randomNumber %></p>
```

But there's no way to load this right now. Also how to pass randomNumber?
Q. Which file could we go to to add this path?

In routes/index.js

```js
router.get('/random', (req, res, next) => {
  // calc random number
  const someNumber = Math.random();

  // display page and pass data
  res.render('random', {
    title: 'random',
    randomNumber: someNumber
  });
});
```

* could have used same variable name for randomNumber
* Assignment 1 asks for 5 views

# Creating partials

* Inside of views/ create partials/ folder (similar to PHP)
* Inside of partials/ create header.ejs and footer.ejs
* Q. What should I remove? Cut and paste appropriate html sections from index.ejs and paste into header and footer
* in index.ejs, replace what you cut with <% include partials/header %> and <% include partials/footer %>

# Add a CSS framework

* I'm going to use getmdl.io
* For your lab I've given you another option to use for bonus marks. No bootstrap.

```html
<link rel="stylesheet" src="/stylesheets/whatever.css" />
```

* you need the leading slash before stylesheets

# Deployment

* Upload current code to Github /comp2068-lesson4-part2
* most internet hosting companies don't support node. Only cloud services do (Digital Ocean, AWS, heroku, google, azure)

How is cloud hosting different from traditional hosting?

\*

# What is package-lock.json?

# error msg if running port 3000 twice
