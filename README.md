# Aker-Systems-Project-3-MERN-Stack-Implementation

## Intro

In this project I'm going to implement a MERN stack web application consisting off the following:
- MongoDB  -- The database
- ExpressJs -- backend
- ReactJS -- backend
- Node.js -- front-end/client-side

This will be done using an aws EC2.

## Backend Configuration

Firstly I run the series of command below to update and upgrade my OS, along with installing NodeJS, and verifying the installation.

```
# Ubuntu (20.04) ec2
sudo apt update
sudo apt upgrade

# get location of node.js from ubuntu repository, amd install
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

# Check installation
sudo apt-get install -y nodejs
node -v 
npm -v
```

![alt text](/node.png)

We next move on to create our application code, which involves creating a directory, 'Todo', and initialise the project via `npm init`.

![alt text](/initialize_.png)

## ExpressJS Installation

Express helps us define routes of our application based on HTTP methods and URLs. We first need to install express and dotenv, along with create and index.js file.

```
npm install express
touch index.js
npm install dotenv
```

Index js we place the following

```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

![alt text](/expressjs.png)

We can now test the server through `node index.js`. Change the security groups in our ec2 to allow port 5000, and check the browser.

![alt text](/port5000.png)
![alt text](/port5000_result.png)


Next we are moving on to creating 'Routes', since our application should allow us to create a task, view all tasks, and delete completed tasks. They will be associated with POST,GET,DELETE request. I create a folder called 'routes' in which i create a file called 'app.js' and place the following below.

```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

## Models

The schema and model, need to be created. The following commands are run to install mongoos as well as create a models folder of which I place a todo.js.

```
npm install mongoose
mkdir models && cd models && touch todo.js
```

In todo we create the schema and model. We then update api.js with the new routes.

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```

New Routes (api.js)
```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');
```


## MongoDB

I create an account on mlab so i can use mongodb as a service. Afterwhich i create the DB and user. A .env file is created that contains the DB link, as an environment variable.

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

The index.js is updated to reflect this next change.

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```

- After starting the server the following result is seen below.

![alt text](/successful_database_connection.png)


## Frontend Configuration

```
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},

```


import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>
          Edit <code>src/App.js</code> and save to reload.
        </p>
        <a
          className="App-link"
          href="https://reactjs.org"
          target="_blank"
          rel="noopener noreferrer"
        >
          Learn React
        </a>
      </header>
    </div>
  );
}

export default App;
