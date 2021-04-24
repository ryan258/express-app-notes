# Express Essentials Course

[Linkedin Learning Course](https://linkedin.com/learning/express-essential-training)

## Express Generators

To set up the scaffolding of an express project.

```terminal
<!-- instead of installing things locally use npx -->
$ npx express-generator

<!-- start off by seeing you generator options -->
$ npx express-generator -h

<!-- then configure your setup with flags to set up an express skeleton that is set to use handlebars, sass, and includes a .gitignore file -->
$ npx express-generator --view=hbs --css=sass --git

$ npm i
$ DEBUG=myapp:* npm start

<!-- then go to http://localhost:3000/ -->
```

**Howeverm in this course we'll be learning the ins and outs by building our express app from scratch.**

## Routes

The bread and butter of any application are routes.

- They provide you with what data you requested
  - **GET** - gets data from the server
  - **POST** - posts data to the server
  - **PUT** - basically an update to something on the server
  - **DELETE** - delete something off the server
- They follow a pattern of request and response
  - **request** - the link you requested along with the method
  - **response** - provides with the data
- the **handler** is the block of code that runs when the route is called

### Static Paths

**app.use()** - allows you to apply specific middleware to a path

**express.static()** - lets you set up a path to a directory where you want to keep your static files

```js
// Examples

// this is for the public folder on path /
app.use(express.static('public'))
// this is for images folder on path images
app.use('/images', express.static('images'
```

## Routing with Express

### Routing Parameters

```js
app.get('/item/:id', (req, res) => {
  // the params will be on the req object
  console.log(req.params.id)
  // it's a string coming from a param and we need it to be a number for when we match it to an id in a DB
  let user = Number(req.params.id)
  console.log(user)
  // so now we'll dig into a JSON file and grab the object in that array position
  console.log(data[user])
  // we'll send that JSON obj to the browser
  res.send(data[user])
})
```

You can also have multiple params in a single path

```js
app.get('/item/:category/:id', (req, res) => {...})
```

### Route Handlers

Route handlers is the block of code that happens inside of your route.

**next()** - A neat express function that allows us to handle multiple handlers in a single route call.

- and you can do it multiple times in the same call
- so this allows you to create your own middleware to do anything you'd like before or after

**BUT NOTE that you can only do one response method for a single call**

```js
app.get(
  '/item/:id',
  (req, res, next) => {
    console.log(req.params.id)
    let user = Number(req.params.id)
    console.log(user)
    console.log(data[user])
    res.send(data[user])
    // next allows us to go to the next callback function
    next()
  },
  (req, res) => {
    // next() jumped us into this function to continue running from 1 function to the next
    console.log('did you get the right data?')
  }
)
```

## Useful Tools

- [mockaroo.com - for mocking up API data](https://www.mockaroo.com/)
