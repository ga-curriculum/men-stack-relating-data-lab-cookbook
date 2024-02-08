# ![MEN Stack Relating Data Lab - Cookbook - Exercise](./assets/hero.png)

**Learning Objective:** By the end of this lab, students will be able to build a full-stack application using the MEN Stack Session Auth Template, with a focus on managing a user's pantry through an embedded data model.

## Introduction

In this lab, you will create a "Cookbook" application where users can manage their pantry items. You'll start with the MEN Stack Session Auth Template which includes a User model with built-in session authentication. 

**MVP: Your task is to add a `pantry` as an embedded data model within the User model.**

## Step 1: User stories and application planning

Kick off your planning by defining clear user stories. These stories guide the functionalities and features of the Cookbook app, ensuring a user-centered design approach. 

Consider the following examples:  [tktk refine the language around pantry items vs foods]

  - As a User (AAU), I want to be able to sign up and sign in to my account to access my user profile.
  - AAU, I want to click on a 'My Pantry' link to view all items in my pantry.
  - AAU, I want to click on a 'Add to Pantry' link where im taken to a new page with a form for adding items to my pantry
  - AAU, I want to submit the pantry item form, saving the food item in my pantry.
  - AAU, I want to update and delete items in my pantry to manage my pantry effectively.
  - AAU, I want to view a list of all app users.
  - AAU, I want to click on a user and view all items in their pantry.

## Step 2: Data modeling and ERD creation

Your next step involves structuring your data.  

1. **Food Schema:** You will be creating a `Food` as an embedded array in the User model, representing the user's `pantry`. [tktk refine pantry vs food]

An ERD will help visualize these relationships:

![Cookbook App ERD](../assets/erd.png)


## Food Schema

Each user's pantry will contain an array of `foods`, each defined by a simple schema:

[tktk add a table and leave code blank] 
 
```js
const foodSchema = new mongoose.Schema({
  // define properties of food schema
});

const userSchema = new mongoose.Schema({
  username: {
    type: String,
    required: true,
  },
  password: {
    type: String,
    required: true,
  },
  pantry: [foodSchema], // this is where we embed our data 
});
```

## Step 3: RESTful routes for managing data

Employ RESTful routing principles to manage foods in the pantry:

| Action   | Route                       | HTTP Verb  |
|----------|-----------------------------|------------|
| Index    | '/foods'                    | GET        |
| New      | '/foods/new'                | GET        |
| Create   | '/foods'                    | POST       |
| Show     | '/foods/:itemId'            | GET        |
| Edit     | '/foods/:itemId/edit'       | GET        |
| Update   | '/foods/:itemId'            | PUT        |
| Delete   | '/foods/:itemId'            | DELETE     |


## Building the Application


### 1. Creating our foods controller

- Create `foods.js` in the controllers directory.
- Include essential modules and define a router.
- Export the router for use in `server.js`.

```js
// controllers/foods.js

const express = require('express');
const router = express.Router();

const User = require('../models/user.js');

// we will build out our router logic after the import of the user model and above our module.exports line

module.exports = router;
```

### 2. Connect controller to server

- Import the applications controller in `server.js`.
- Use middleware to link the controller to a specific route.

```js
// server.js

const foodController = require('./controllers/foods.js');
```

### 3. Stubbing controller routes

- Start by creating a `GET` route for `/foods` (index route).
- `res.send()` a simple string confirming the route was hit.
- Test the index route in a browser or using `postman`.
- Repeat this process for all of your controller routes.

Use the route table as your guide:

| Action   | Route                       | HTTP Verb  |
|----------|-----------------------------|------------|
| Index    | '/foods'                    | GET        |
| New      | '/foods/new'                | GET        |
| Create   | '/foods'                    | POST       |
| Show     | '/foods/:itemId'            | GET        |
| Edit     | '/foods/:itemId/edit'       | GET        |
| Update   | '/foods/:itemId'            | PUT        |
| Delete   | '/foods/:itemId'            | DELETE     |


## Creating the folder and middleware

[tktk determine what is needed here ] 

## Building your views

Use the following steps as you build out each `view` and its corresponding route handler. 

### 1. Start with `New`

1. **Conceptualize the Route**

  - Recall user stories for context on view requirements:
    - AAU, I want to click on an 'Add Food' link that takes me to a `new` page for adding new food items with a form
    - AAU, I want to submit the form, saving the food item in my pantry.
  - Use RESTful conventions for the route: `GET /users/:userId/foods/new`.

2. **Define the Route on the Server**

    - In `controllers/foods.js`, update the `new` route.
    - Use `res.render()` to display a `new.ejs` view.

3. **Create the `View` for the `New` Page**

    - In `views/foods`, create `new.ejs`.
    - Add basic HTML structure for the page
    - Build a form in the view for saving new `foods`, use the `food` schema as your guide when building your form

4. **Build the Create Functionality**
    - Refactor the `POST` route to handle `CREATE` functionality for data sent from the form

To get you started: 

```js
// controllers/foods.js`

router.post('/', async (req, res) => {
  try {
    //find the current user
    const currentUser = await User.findById(req.session.user._id);
    //add the new item to the users enbeded pantry array
    currentUser.pantry.push(req.body);
    //save changes to the user
    await currentUser.save();
    // redirect to index page
    res.redirect(`/users/${currentUser._id}/foods`);
  } catch (error) {
    console.log(error);
    res.redirect('/')
  }
});
```

### Index [tktk]

Using the Steps outlined above, Implement the following User Story:

- AAU, I want to click on a 'My Pantry' link to view all items in my pantry.

### Show [tktk]

### Delete [tktk]

### Edit [tktk]

### All Users [tktk]