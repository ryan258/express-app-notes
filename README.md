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
