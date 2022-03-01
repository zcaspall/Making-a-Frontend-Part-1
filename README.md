# Making a Frontend: Part 1

The starter code is based on the solution code for the last homework assignment.

You will be making some new directories and files so check the directory structure at the end of the instructions to make sure you're on the right track.

## Adding static files

So far the backends have been serving HTTP status codes and/or JSON responses. However, now we want our server to serve documents. For now we will use **static assets**. A static assets are resources that do not change. In other words we will create the documents and serve the exact same version every time a request is sent. 

Create a new directory in the root of your project directory called `public`. Now create another directory inside of `public/` called `images`. To simplify things we will add HTML documents to the root of `public/`

You will put any files that are meant to be accessed by the frontend inside of the `public/` directory in the appropriate folder. However, the client still can't access these files because we haven't added access to the server's middleware chain.

## images

Download the file `notWordle.png` from the class note section on cscade and add it to the `images` directory.

## HTML

Now you can create your HTML files. Add each of them to the `public/` directory. Remember to use fully compliant HTML markup. Use the `html:5` emmet completion in VS code.

### `index.html`

This is a specially named file. `index.html` is the "homepage" for the website. It will be served when the user visits `http://localhost:8080`. You can call it anything you want; however, the convention is to call it `index.html`.

This file will be rather simple. The `title` should be "Welcome To Not Wordle".

The `body` should contain an `img` tag containing `notWorlde.png` (which is in the images directory) and a level one header with the text "Welcome to Not Wordle!". Then include a link using the `a` (anchor) tag to `/game` using the `href` attribute and the text `"Play the game!"`.

Links to docs:

- [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img)
- [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)

**This is not HTML! It is just an example of the nesting.**
```
body
    img
    h1
    a
```

### Expected Page

![Game Page HTML](https://github.com/csaldivar-astate/webDev-images/blob/main/makingTheFrontEnd-part1/notWordleIndex.png?raw=true)

### `game.html`

The `title` should be `"Not Wordle"`.

This is the main game page and should contain the following:
- an unordered list of 6 guesses all containing the text `?????`
- a form, with `method` attribute set to `POST` and an `action` attribute set to `/api/guess`
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `guess`
    - a text input with the `name` attribute set to `guess`, and the `required` boolean attribute
  - a button with the text `Submit`

**This is not HTML! It is just an example of the nesting.**
```
body
    ul
        li
        li
        li
        li
        li
        li
    form 
        div
          label
          input
        button
```

The browser will submit the `POST` request to `/api/guess` when you hit the submit button. Notice how the page reloads when you hit submit? That is the default browser behavior. We can disable it with JavaScript later.

The more pressing issue is that the server won't be able to parse the body of this request! This is because HTML forms use `urlencoded` messages bodies for POST requests and the server only knows how to parse JSON encoded message bodies. So you'll need to enable `urlencoded` parsing. You'll fix this later.

### Expected Page

![Game Page HTML](https://github.com/csaldivar-astate/webDev-images/blob/main/makingTheFrontEnd-part1/game.png?raw=true)

Links to docs:

- [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)
- [https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)


### `register.html`

The `title` should be `"Register for Not Wordle"`.

This is the registration page where we allow user's to create accounts.

- a form, with `method` attribute set to `POST` and an `action` attribute set to `/register`
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `username` and the text `Username`
    - a text input with the `name` attribute set to `username`, and the `required` boolean attribute
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `password` and the text `Password`
    - a text input with the `name` attribute set to `password` and `type` set to `password`, and the `required` boolean attribute
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `confirmPassword` and the text `Confirm Password`
    - a text input with the `name` attribute set to `confirmPassword` and `type` set to `password`, and the `required` boolean attribute
  - a button with the text `Submit`

**This is not HTML! It is just an example of the nesting.**
```
body
    form 
        div
          label
          input
        div
          label
          input
        div
          label
          input
        button
```


### Expected Page
![Register Page HTML](https://github.com/csaldivar-astate/webDev-images/blob/main/makingTheFrontEnd-part1/register.png?raw=true)

### `login.html`

The `title` should be `"Log in to Not Wordle"`.

This is almost the registration page where we allow user's to create accounts and now they can log in instead.

- a form, with `method` attribute set to `POST` and an `action` attribute set to `/login`
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `username` and the text `Username`
    - a text input with the `name` attribute set to `username`, and the `required` boolean attribute
  - a div that will contain the following elements
    - a label tag with the `for` attribute set to `password` and the text `Password`
    - a text input with the `name` attribute set to `password` and `type` set to `password`, and the `required` boolean attribute
  - a button with the text `Submit`

**This is not HTML! It is just an example of the nesting.**
```
body
    form 
        div
          label
          input
        div
          label
          input
        button
```

### Expected Page
![Login Page HTML](https://github.com/csaldivar-astate/webDev-images/blob/main/makingTheFrontEnd-part1/login.png?raw=true)

## Enabling `urlencoded` parsing

In `app.js` add the following immediately before or immediately after you enabled json parsing.

```js
app.use(express.urlencoded({ extended: false }));
```

Now your server can parse `urlencoded` messages bodies. So your forms work! Express will add the data to the `req.body` object exactly like before. The `name` attribute for each element acts as the value's key. So you don't need to change anything else in your server code for request bodies to work.

## Serving static files

Because these assets are static, we don't really need to use route handlers and complex code. Rather, the server will just give the client access to the files directly. However, we need to turn this feature on. You can use the `express.static()` middleware. Link to docs: [https://expressjs.com/en/api.html#express.static](https://expressjs.com/en/api.html#express.static).

Add this **before** you enable json parsing. 

```js
app.use(express.static("public", {
    index: "index.html",
    extensions: ['html']
}));
```

Whenever you click a link or navigate to a web page the browser will send a `GET` request to that path. For example if you clicked the link: [localhost:8080/register](localhost:8080/register). Your browser would send an HTTP request for `GET /register`. I do not want this to go to a route handler, in other words `GET /register` is not and API endpoint. Instead I want the server to respond with the document `register.html`.

Since this is the first function in the middleware chain it will execute first. If it finds a page matching "register.html" it will serve the file instead of allowing execution to reach the endpoints. However, if a request for `/api/guess` was received then this middleware function wouldn't find an `api/` directory and just pass control on to the next function in the stack.

The option `extensions: ['html']` makes it so that our URLs won't look like: `localhost:8080/register.html` and instead they'll be `localhost:8080/register`.

Ensure your pages are accessible by going to the URLs:
- [localhost:8080/](localhost:8080/)
- [localhost:8080/register](localhost:8080/register)
- [localhost:8080/login](localhost:8080/login)
- [localhost:8080/game](localhost:8080/game)

## Final Directory Structure

By the end of the assignment this should be your directory structure (`package-lock.json`, `node_modules/`, `tests/`, `.git/` and `.vscode/` are not listed)

```
.
├── Database
│   ├── init-db.js
│   ├── schema.sql
│   └── wordle.db
├── Models
│   ├── db.js
│   └── dictionaryModel.js
├── public
│   ├── images
│   │   └── notWordle.png
│   ├── game.html
│   ├── index.html
│   ├── login.html
│   └── register.html
├── README.md
├── app.js
├── package.json
└── server.js
```