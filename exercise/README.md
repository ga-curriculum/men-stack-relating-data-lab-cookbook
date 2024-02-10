# ![MEN Stack Relating Data Lab - Cookbook - Exercise](./assets/hero.png)

## Introduction

In this lab, you will create a "Cookbook" application where users can manage items in their pantry for use in recipes. You'll start with the MEN Stack Session Auth Template which includes a User model with built-in session authentication. 

**MVP: Your task is to add a `pantry` as an embedded data model within the User model.**

## Step 1: User stories and application planning

Begin planning by defining clear user stories. These stories guide the functionality and features of the app using a user-centered design approach. 

Consider the following examples:

  - As a User (AAU), I want to sign up for an account and sign in, so that I can access my personal user profile and manage my pantry.
  - AAU, I need a dedicated page to view all items in my pantry, to easily manage and review what I have stored.
  - AAU, I want to easily find and click on an 'Add New Item' link, which takes me to a form for adding new items to my pantry
  - AAU, after filling out the pantry item form, I want to submit it and receive confirmation that the item has been saved in my pantry
  - AAU, I need the ability to edit and delete items in my pantry, allowing me full control over managing my stored items
  - AAU, I am interested in viewing a list of all other app users, to foster a sense of community and interaction within the app.
  - AAU, I want the option to click on another user's profile and view all the items in their pantry, to exchange ideas or find inspiration for my own pantry.

## Step 2: Data modeling and ERD creation

Your next step involves structuring your data.  

1. **Food Schema:** Your task is to create a `Pantry`, an embedded array within the `User` model. This array represents the user's collection of food items. Because the pantry holds foods, the name of our schema will be `foodSchema`.

An ERD will help visualize this relationship:

![Cookbook App ERD](../assets/erd.png)


### Create food schema

Each user's pantry will contain an array of `foods`, each defined by a simple schema:

| Property        | Type     | Required  |
|-----------------|----------|-----------|
| name            | String   | true      |

Begin by defining `foodSchema` in the `user` model file using the `mongoose.Schema` method. Include the necessary properties for food items.

```js
// user.js

const foodSchema = new mongoose.Schema({
  // define properties of food schema
});
```

### Embed `foodSchema` in `userSchema` 

Next, modify `userSchema` to include the `foodSchema`. Do this by adding a property named `pantry`. This property will utilize the `foodSchema` to represent a list of food items associated with a user.

```js
const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
  },
  password: {
    type: String,
    required: true,
  },
  pantry: // Insert foodSchema here
});
```

## Step 3: Define RESTful routes for managing data

Use this chart of RESTful routes as a guide for building out your application.

| Action   | Route                                     | HTTP Verb  |
|----------|-------------------------------------------|------------|
| Index    | '/users/:userId/foods'                    | GET        |
| New      | '/users/:userId/foods/new'                | GET        |
| Create   | '/users/:userId/foods'                    | POST       |
| Show     | '/users/:userId/foods/:itemId'            | GET        |
| Edit     | '/users/:userId/foods/:itemId/edit'       | GET        |
| Update   | '/users/:userId/foods/:itemId'            | PUT        |
| Delete   | '/users/:userId/foods/:itemId'            | DELETE     |

Because this data is embedded in the user model, our routes need to be structured to reflect that relationship.

## Building the Application

### 1. Build the controller

- Create a file called `foods.js` in the controllers directory.
- Require all essential modules at the top of `foods.js`
- Export your router for use in `server.js`

```js
// controllers/foods.js

const express = require('express');
const router = express.Router();

const User = require('../models/user.js');

// todo: build out your router logic here

module.exports = router;
```

### 2. Use the controller in server

- Import the foods controller in `server.js`.
- Use middleware to direct incoming requests to `/users/:userId/foods` to the foods controller.


```js
// server.js

const authController = require('./controllers/auth.js');
// below auth controller import
const foodsController = require('./controllers/foods.js');
```

```js
// server.js

app.use('/auth', authController);
// below use auth controller
app.use('/users/:userId/foods',foodsController);
```

## Add middleware

Now you'll need to add two middleware functions to your application:

- one that restricts access to logged-in users only
- one that makes the user object available to view templates via `res.locals`. 

Create a `middleware` folder in your project root. Inside this folder:

- Create a file named `is-signed-in.js`.

- This function should include logic that checks if `req.session.user` exists, and if it does, it allows the request to continue on the normal chain by invoking `next()`

```js
// is-signed-in.js

const isSignedIn = (req, res, next) => {
  if (req.session.user) return next();
  res.redirect('/auth/sign-in');
};

module.exports = isSignedIn;
```

- Create a file named `pass-user-to-view.js`.

- This function should include logic that that assigns `req.session.user` to `res.locals.user`. If no user is found, we set it to `null`. Then allow the request to continue on the normal chain by invoking `next()`.

```js
// middleware/pass-user-to-view.js

const passUserToView = (req, res, next) => {
  res.locals.user = req.session.user ? req.session.user : null
  next()
}

module.exports = passUserToView
```

Import and include both middleware functions above all of the routes and controllers in `server.js.`.

```js
// server.js
const isSignedIn = require('./middleware/is-signed-in.js');
const passUserToView = require('./middleware/pass-user-to-view.js');
```

These middleware functions should run before any routes that require a user to be logged in or require access to a user object in the view. 

For this application, users must be signed in to view any of the routes associated with their pantry. Therefore, it makes sense to include them directly above the `foods` controller, but not before `auth`.

```js
app.use('/auth', authController);
app.use(isSignedIn);
app.use(passUserToView);
app.use('/users/:userId/foods',foodsController);
```

You are now ready to begin building your protected routes. 

## Build the Landing page 


### Conceptualize the `index` route

For your landing page, you will use:

   - `GET` `/users/:userId/foods`

### Create the `index` route

- In your `foods` controller, create a new `index` route to serve as the landing page. Test by visiting `localhost:3000/users/foods`.

```js
router.get('/', (req, res) => {
  // your route logic here
});
```

### Render the `index` template

- Create a new directory called `foods` inside of the `views` directory. This will hold the templates for your `foods` pages. 

- Create an `index.ejs` file in `foods`. Add basic HTML boilerplate to this file.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Pantry</title>
  </head>
  <body>
    <h1>Welcome to my Pantry</h1>
  </body>
</html>
```

- Adapt your controller's logic to use `res.render` for displaying the `index.ejs` template.

```js
res.render('foods/index.ejs')
```

- Test your new route in the browser to confirm that everything renders correctly.


## Build the `nav` partial

## Build the `new` page

## Build the `create` functionality

## Build the `new` page

## Add `index` to Landing page

## Build the `show` page

## Build the `delete` functionality 

## Build the `edit` page

## Build the `update` functionality 
