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

### Common Routing Methods

There are multiple response types we can do like .send() and .json() to send responses to the browser.

We can also render pure HTML by coding it directly into our response.

**.json()** sends a data response to the client, ready to be consumed by front end code

**.end()** ends a call right away, but usually you won't have a need for this

```js
app.get('/item', (req, res) => {
  // ends the call right away
  res.end()
  res.send(`a get request with /item route on port ${PORT}`)
  // ^^ the res.send() never runs
})
```

**.redirect()** sends them to a given url

```js
app.get('/item', (req, res) => {
  // redirect to another page
  res.redirect('https://www.linkedin.com')
})
```

**.download()** responds with a download

```js
app.get('/images', (req, res) => {
  // respond to /images with a download
  res.download('images/rocket.jpg')
})
```

For more routing methods [visit the express docs](http://expressjs.com/en/guide/routing.html)

### Chaining Routes

As your API grows you may need to refactor your code and chaining multiple methods into a single block of code for a specific path can do that.

1. Get similar calls on the same paths

## Express & Middleware

### Basic Middleware w/ Express

**Middleware** is simply functions that have access to the request and response cycle in an Express app and can run their code there.

- It can make changes to the request and response object
- Can call another function in the stack w/ .next()
- Basically adding own code before sending a response back

**express.JSON()** allows us to parse JSON POST commands, meaning that when a form or the front end wants to send new data to the server and databaseand send it in JSON, we need our server to be able to use it in this format and then POST it to the database.

- It's Express' version of body-parser

```js
// method to use JSON
//app.use(express.json());
app.use(express.urlencoded({ extended: true }))

// this is for images folder on path images
app.use('/images', express.static('images'))

app.get('/', (req, res) =>
  // get data first
  res.json(data)
)

// JSON data
// { "hello": "JSON is cool"}

// URLEncoded data
// hello=URLEncoded+is+cool
```

### Error Handling

Handling errors must be last in your middleware logic after app.use() and route calls. Put it after the last app.route() or app.listen()

If there's an error Express will look for a custom error handler and if there isn't one it will fallback on its stack error handling.

```js
app
  .route('/item')
  .get((req, res) => {
    // res.send(`a get request with /item route on port ${PORT}`)
    throw new Error()
  })
  .post((req, res) => res.send(`a post request with /item route on port ${PORT}`))
  .put((req, res) => res.send(`a put request with /item route on port ${PORT}`))
  .delete((req, res) => res.send(`a delete request with /item route on port ${PORT}`))

// Error Handling Function
// if we don't express our error handling function Express will fall back on its stock error handling.
app.use((err, req, res, next) => {
  console.error(err.stack)
  // customize the error handling
  res.status(500).send(`Red alert! ${err.stack}`)
})

app.listen(PORT, () => {
  console.log(`Your server is running on port ${PORT}`)
  console.log(data)
})
```

### 3rd Party Middleware

- [Express Middleware list on expressjs.com](http://expressjs.com/en/resources/middleware.html)
- [serve-favicon middleware](http://expressjs.com/en/resources/middleware/serve-favicon.html)

For this example we'll use the serve-favicon middleware.

```cmd
$ npm i serve-favicon
```

Import the middleware into index.js

```js
import favicon from 'serve-favicon'
import path from 'path'
```

Use serve-favicon to serve our favicon

```js
// use serve-favicon
app.use(favicon(path.join(__dirname, 'public', 'favicon.ico')))
```

Then you may want to clear the browser cache.

## Advanced Topics in Express

### Debugging in Express

Setting up debugging in Express is actually very easy.

From the terminal in your code editor
**For Mac or Linux**

```terminal
$ DEBUG=express:* node index.js
```

**For Windows**

```terminal
$ set DEBUG=express:* & node index.js
```

But you can also just set this up in your package.json file.

```json
"scripts": {
    "start": "nodemon ./index.js --exec babel-node -e js",
    "debug": "DEBUG=express:* nodemon ./index.js --exec babel-node -e js",
    "debug-windows": "set DEBUG=express:* & nodemon ./index.js --exec babel-node -e js"
  },
```

### Express Behind a Proxy

[Express behind proxies docs](https://expressjs.com/en/guide/behind-proxies.html)

A **proxy** is basically another server that pushes endpoint calls or traffic to your app.

Setting up proxies in Express is easy! It's just a single line with options and args.

But there are a lot of things and concerns that go into it to maintain proper security. So if you're going to use them, make sure you review some security courses on Node.js and Express.

```js
// whatever IP addresses you decide to use, you need to make sure they're secure
app.set('trust proxy', 'loopback, 123.123.123.123') // specify a single subnet
```

### Security Overview for Express - Best Practices

- Always keep dependencies up to date and secure
  - follow official lists promoted by the Express team or internal ones
  - if any of them are depricated, remove them or find options
- if using sensetive data, use TLS (transport layer security) which is the next progression of SSL
- use Helmet's collection of security middleware
- use cookies securely
  - as a base use the Express session middleware when you want to manage cookies in your app
- Look at [Node Security Checklist](https://blog.risingstack.com/node-js-security-checklist/) and stay informed of known issues

### Real-time Web w/ Express

[socket-io](https://socket.io/)

Socket uses an event-based approach for syncing and transmitting data in real-time.

- it uses events
- then subscribes to those events

If some one types a message in a chatroom, then everyone in that chatroom is updated with that message information.

[See the demo section](https://socket.io/demos/chat/)

There's also a LL course called Node.js: Real-Time Web w/ Socket.io
It's also worth learning about JWT - security features for APIs

## Useful Tools

- [mockaroo.com - for mocking up API data](https://www.mockaroo.com/)
- [letsencrypt - get a free TLS certificate](https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/)

## Other Options

- Koa
- Hapi
- Sails
