# Interview Prep Kit (Junior Developer)

## Django Setup

- [ ] Make a directory for the project
- [ ] Run `pip3 install virtualenv` to install a virtual environment
- [ ] Run `python3 venv .env` to initialize your virutal environment
- [ ] Run `source .env/bin/activate` to activate your virtual environment. Make sure to have this active while working on the project and run all commands should use this. 
- [ ] Install django using `pip install django` (you may combine this with installing psycopg2 below)
- [ ] Install psycopg2-binary using `pip install psycopg2-binary` 
- [ ] Create a new project using `django-admin startproject PROJECTNAMEHERE .`. Make sure to change the name of the project to something relevant. You should now find a manage.py file at the root of your directory. If you don't see it you probably didn't copy the . in the command. 
- [ ] Create an app instance using `django-admin startapp APPNAMEHERE`. Make sure to name your app something distinct from your project. 
- [ ] Create any other app instances you'd like or need using `django-admin startapp APPNAMEHERE`.

## DB Setup (Postgresql)

- [ ] Run `psql -d postgres` to open your sql cli
- [ ] Create a new database inside the cli using `CREATE DATABASE dbname;`. Be sure to replace dbname with a relevant name. 
- [ ] Create a user and assign it a password using `CREATE USER username WITH PASSWORD 'password';`. Make sure to change the username and password with the actual username and password you would like. Remember these as they will be necessary later. 
- [ ] Grant all privileges to the created user using `GRANT ALL PRIVILEGES ON DATABASE dbname TO username`. Make sure to replace dbname and username with the values you entered in the commands above. 
- [ ] 
- [ ] Export your setup mongoose: `module.exports = mongoose`

Debugging notes: There is also no need to capitalize "CREATE DATABASE" or any other subsequent command while in this CLI. If successful you should see a response from SQL most of the time. Make sure to use a semicolon at the end (this is true for all sql commands for the most part). 

## Define a Schema + Model

- [ ] Create a file with your model's name in `models/`
- [ ] Import mongoose from your connection file: `const mongoose = require("../db/connection")`
- [ ] Create your schema and set it to a variable.
```js
const mySchema = new mongoose.Schema({
  field: DataType,
})
```
- [ ] Create a model with your schema: `const myModel = mongoose.model("modelName", mySchema)`
- [ ] Export your model: `module.exports = myModel`

## Seed your DataBase

- [ ] Create `db/seed.js`
- [ ] Import your schema from your model file: `const myModel = require('../models/myModel')`
- [ ] Create your seed data in JSON format in `db/seeds.json`
- [ ] Import your seed data into your `seeds.js` file: `const seedData = require("./seeds.json")`
- [ ] Remove all of the items currently in your database: `myModel.remove({})`
- [ ] Insert your seeds into your database within the `.then` method.
- [ ] Exit the seeding process in another `.then` method.
```js
myModel.remove({})
  .then(() => {
    return myModel.collection.insert(seedData)
  })
  .then(() => {
    process.exit()
  })
```
- [ ] Run your seed file in the terminal: `$ node db/seed.js`

## Set Up your Controller

- [ ] Create a controller file: `controllers/myItems.js`
- [ ] Require your controller in `index.js`: `const myController = require('./controllers/myController')`
- [ ] Use your controller **below any other configuration**: `app.use("/myUrlPrefix", myControllerController)`
- [ ] Require Express in your controller file: `const express = require("express")`
- [ ] Create a router instance in your controller file: `const router = express.Router()`
- [ ] Import your model: `const myModel = require('../models/myModel')`
- [ ] Export your router instance at the very bottom of the file: `module.exports = router`

## Create your Index Route

- [ ] Setup a GET handler for the '/' route: `router.get("/", (req, res) => {})`
- [ ] In the body of the GET handler, find all of the instances of your model: `myModel.find({})`
- [ ] Once the items are returned from your database, render your index view: `res.json(myInstances)`
```js
router.get("/", (req, res) => {
  myModel.find({}).then(myInstances => res.json(myInstances))
})
```

## Create your Show Route

- [ ] Setup a GET handler for the '/:id' route: `router.get("/:id", (req, res) => {})`
- [ ] In the body of the GET handler, find an instance of your model: `myModel.findOne({ _id: req.params.id })`
- [ ] Once the items are returned from your database, render your index view: `res.render('show', { myInstance })`
```js
router.get("/:id", (req, res) => {
  myModel.findOne({ _id: req.params.id }).then(myInstance => res.json(myInstance)))
})
```

## Create your New Route

- [ ] Install body-parser: `$ npm install body-parser`
- [ ] Require body-parser in your `index.js`: `const parser = require('body-parser')`
- [ ] Setup body-parser in your `index.js`: `const parser = app.use(parser.urlencoded({ extended: true }))` and `app.use(parser.json())` **note: put this above where you `use` your controllers!**

- [ ] Setup a POST handler for your post reqest to create a new item at url `/`: `router.post("/", (req, res) => {})`
- [ ] In the body of the POST handler, create an instance of your model with the data from the form: `myModel.create(req.body)`
- [ ] Once the items are returned from your database, redirect to your home page: `res.redirect('/')`
```js
router.post('/', (req, res) => {
  myModel.create(req.body)
    .then(myNewItem => {
      res.redirect('/')
    })
})
```

## Create your Update Route

- [ ] Find the item you want to edit in your database: `myModel.findOne({_id: req.params.id})`
- [ ] Render the form: `res.render('edit', { myInstance })`
- [ ] Create a route to edit the item in the database: `router.put('/:id', (req, res) => {})`
- [ ] Find an item in the database and edit it: `myModel.findOneandUpdate({_id: req.params.id}, req.body, { new: true })`
- [ ] Redirect to the home page: `res.redirect('/')`
```js
router.put('/:id', (req, res) => {
  myModel.findOneAndUpdate({_id: req.params.id}, req.body, { new: true })
    .then(myInstance => {
      res.redirect('/')
    })
})
```

## Create your Delete Route

- [ ] Create your delete route: `router.delete(':id', (req, res) => {})`
- [ ] Remove the item from the database: `myModel.findOneAndRemove({ _id: req.params.id })`
- [ ] Redirect to the home page: `res.redirect('/')`
```js
router.delete('/:id', (req, res) => {
  myModel.findOneAndRemove({ _id: req.params.id })
    .then(() => {
      res.redirect('/')
    })
})
```

## CORS

- [ ] `npm install cors`
- [ ] add cors to the `index.js` - `const cors = require('cors')`
- [ ] add the cors middleware - `app.use(cors())`
