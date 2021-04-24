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

**Howeverm in this course we'll be learning the ins and outs by building our express app from scratch.**

## Useful Tools

- [mockaroo.com - for mocking up API data](https://www.mockaroo.com/)
