# shopping-list
connecting nodejs to mongodb
let express = require("express")
let { MongoClient, ObjectId } = require("mongodb")

let app = express()
let db

app.use(express.static("public"))

/*
async function go() {
  let client = new MongoClient("mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.dfvvi.mongodb.net/App?retryWrites=true&w=majority")
  await client.connect()
  db = client.db()
  app.listen(3000)
}

*/
async function go() {
  //mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.dfvvi.mongodb.net/App?retryWrites=true&w=majority
 // let url = 'mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.vug1yz8.mongodb.net/APP' 
let client = new MongoClient('mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.vug1yz8.mongodb.net/APP')
client.connect()
db = client.db()
app.listen(2000)
}
go()

app.use(express.json())
app.use(express.urlencoded({ extended: false }))

app.get("/", async function (req, res) {
  const items = await db.collection("items").find().toArray()
  res.send(`<!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple To-Do App</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
  </head>
  <body>
  <div class="container">
  <h1 class="display-4 text-center py-1">HISMOT APP!</h1>
  
  <div class="jumbotron p-3 shadow-sm">
  <form action="/create-item" method="POST">
  <div class="d-flex align-items-center">
  <input name="item" autofocus autocomplete="off" class="form-control mr-3" type="text" style="flex: 1;">
  <button class="btn btn-primary">ADD TO ME</button>
  </div>
  </form>
  </div>
  
  <ul class="list-group pb-5">
    ${items
      .map(function (item) {
        return `<li class="list-group-item list-group-item-action d-flex align-items-center justify-content-between">
      <span class="item-text">${item.text}</span>
      <div>
      <button data-id="${item._id}" class="edit-me btn btn-secondary btn-sm mr-1">Edit</button>
      <button data-id="${item._id}" class="delete-me btn btn-danger btn-sm">Delete</button>
      </div>
      </li>`
      })
      .join("")}
  </ul>
  
  </div>
  
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
  <script src="/browser.js"></script>
  </body>
  </html>`)
})

app.post("/create-item", async function (req, res) {
  await db.collection("items").insertOne({ text: req.body.item })
  res.redirect("/")
})

app.post("/update-item", async function (req, res) {
  await db.collection("items").findOneAndUpdate({ _id: new ObjectId(req.body.id) }, { $set: { text: req.body.text } })
  res.send("Success")
})

app.post("/delete-item", async function (req, res) {
  await db.collection("items").deleteOne({ _id: new ObjectId(req.body.id) })
  res.send("Success")
})


/*
async function go() {
    //mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.dfvvi.mongodb.net/App?retryWrites=true&w=majority
   // let url = 'mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.vug1yz8.mongodb.net/APP' 
  let client = new MongoClient('mongodb+srv://hismot:5VQt0VT6aQj0IRhx@cluster0.vug1yz8.mongodb.net/APP')
  client.connect()
  db = client.db()
  app.listen(2000)
}


*/

/*
let express = require('express')
let {MongoClient} = require('mongodb')
let app = express()
let db


app.use(express.static('public))

async function go() {
let client = new MongoClient(' ')
await client.connect()
db = client.db()
app.listen(2000)
}

go()
app.use(express.urlencoded({extended: false}))

app.get('/', async function (req, res) {
const items = await db.collection('items').find().toArray()
res.send(`<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple To-Do App</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h1 class="display-4 text-center py-1">To-Do App</h1>
    
    <div class="jumbotron p-3 shadow-sm">
      <form action="/create-item" method="POST">
        <div class="d-flex align-items-center">
          <input name="items" autofocus autocomplete="off" class="form-control mr-3" type="text" style="flex: 1;">
          <button class="btn btn-primary">Add New Item</button>
        </div>
      </form>
    </div>
    
    <ul class="list-group pb-5">
      ${items.map(function(item) {
return`
      <li class="list-group-item list-group-item-action d-flex align-items-center justify-content-between">
        <span class="item-text">${item.text}</span>
        <div>
          <button class="edit-me btn btn-secondary btn-sm mr-1">Edit</button>
          <button class="delete-me btn btn-danger btn-sm">Delete</button>
        </div>
      </li>`
}).join('')}
    </ul>
    
  </div>
  
</body>
</html>`)
})


app.post('/create-item', async function(req, res) {
await db.collection('items').insertOne({text: req.body.item})
res.redirect('/')
})


let express = require('express')
let {MongoClient} = require('mongodb')
let app = express()
let db


/*
async function go() {
let client = new MongoClient(' ')
await client.connect()
db = client.db()
app.listen(2000)
}

go()

*/
/*
let connectionString = 'moasoads'
mongodb.connect(connectString, {userNewParser: true}, function (err, click)
db = clientdb.()
app.listen(2000)
app.use(express.json())
app.use(express.urlencoded({extended: false}))

app.get('/', async function (req, res) {
const items = await db.collection('items').find().toArray()
res.send(`<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple Hismot App</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h1 class="display-4 text-center py-1">To-Do App</h1>
    
    <div class="jumbotron p-3 shadow-sm">
      <form action="/create-item" method="POST">
        <div class="d-flex align-items-center">
          <input name="items" autofocus autocomplete="off" class="form-control mr-3" type="text" style="flex: 1;">
          <button class="btn btn-primary">Add New Item</button>
        </div>
      </form>
    </div>
    
    <ul class="list-group pb-5">
      ${items.map(function(item) {
return`
      <li class="list-group-item list-group-item-action d-flex align-items-center justify-content-between">
        <span class="item-text">${item.text}</span>
        <div>
          <button class="edit-me btn btn-secondary btn-sm mr-1">Edit</button>
          <button class="delete-me btn btn-danger btn-sm">Delete</button>
        </div>
      </li>`
}).join('')}
    </ul>
    
  </div>
  
</body>
</html>`)
})
})

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>


app.post('/create-item', async function(req, res) {
await db.collection('items').insertOne({text: req.body.item})
res.redirect('/')
})
})
app.post("/update-item", function (req, res) {
  console.log(req.body.text)
  res.send("Success")



*/
/*
let express = require('express')
let {MongoClient, ObjectId} = require('mongodb')
let app = express()
let db
*/

/*
async function go() {
let client = new MongoClient(' ')
await client.connect()
db = client.db()
app.listen(2000)
}

go()

*/


/*
let connectionString = 'moasoads'
mongodb.connect(connectString, {userNewParser: true}, function (err, click)
db = clientdb.()
app.listen(2000)
app.use(express.json())
app.use(express.urlencoded({extended: false}))

app.get('/', async function (req, res) {
const items = await db.collection('items').find().toArray()
res.send(`<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple Hismot App</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h1 class="display-4 text-center py-1">To-Do App</h1>
    
    <div class="jumbotron p-3 shadow-sm">
      <form action="/create-item" method="POST">
        <div class="d-flex align-items-center">
          <input name="items" autofocus autocomplete="off" class="form-control mr-3" type="text" style="flex: 1;">
          <button class="btn btn-primary">Add New Item</button>
        </div>
      </form>
    </div>
    
    <ul class="list-group pb-5">
      ${items.map(function(item) {
return`
      <li class="list-group-item list-group-item-action d-flex align-items-center justify-content-between">
        <span class="item-text">${item.text}</span>
        <div>
          <button data-id="${item._id}" class="edit-me btn btn-secondary btn-sm mr-1">Edit</button>
          <button data-id="${item._id}" class="delete-me btn btn-danger btn-sm">Delete</button>
        </div>
      </li>`
}).join('')}
    </ul>
    
  </div>
  
</body>
</html>`)
})
})

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>


app.post('/create-item', async function(req, res) {
await db.collection('items').insertOne({text: req.body.item})
res.redirect('/')
})
})
app.post("/update-item",async function (req, res) {
await db.collection('items').findOneAndUpdate({_id: new ObjectId(req.body.id)}, {set: {req.body.text}}})
  res.send("Success")
})
})

app.post('/delete-item',async function(req, res) {
await db.collection('items').deleteOne({_id: new ObjectId(req.body.id)
res.send("Success")
})
})

go()

app.use(express.urlencoded({extended: false}))

app.get('/', async function (req, res) {
const items = await db.collection('items').find().toArray()
res.send(`<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simple Hismot App</title>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h1 class="display-4 text-center py-1">Hismot App</h1>
    
    <div class="jumbotron p-3 shadow-sm">
      <form action="/create-item" method="POST">
        <div class="d-flex align-items-center">
          <input name="items" autofocus autocomplete="off" class="form-control mr-3" type="text" style="flex: 1;">
          <button class="btn btn-primary">Add New Item</button>
        </div>
      </form>
    </div>
    
    <ul class="list-group pb-5">
      ${items.map(function(item) {
return`
      <li class="list-group-item list-group-item-action d-flex align-items-center justify-content-between">
        <span class="item-text">${item.text}</span>
        <div>
          <button class="edit-me btn btn-secondary btn-sm mr-1">Edit</button>
          <button class="delete-me btn btn-danger btn-sm">Delete</button>
        </div>
      </li>`
}).join('')}
    </ul>
    
  </div>
  
</body>
</html>`)
})


app.post('/create-item', async function(req, res) {
await db.collection('items').insertOne({text: req.body.item})
res.redirect('/')
})


*/
