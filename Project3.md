# SIMPLE TO-DO APPLICATION ON MERN WEB STACK
In this project, you are tasked to implement a web solution based on MERN stack in AWS Cloud.
# MERN Web stack consists of following components:
MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
ExpressJS: A server side Web Application framework for Node.js.
ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f470f1bb-9aa6-4437-a73f-f1f3626bd1e7)
As shown on the illustration above, a user interacts with the ReactJS UI components at the application front-end residing in the browser. This frontend is served by the application backend residing in a server, through ExpressJS running on top of NodeJS.
Any interaction that causes a data change request is sent to the NodeJS based Express server, which grabs data from the MongoDB database if required, and returns the data to the frontend of the application, which is then presented to the user.
# Step 0 – Preparing prerequisites
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
Hint #1: When you create your EC2 Instances, you can add Tag “Name” to it with a value that corresponds to a current project you are working on 
# Task
To deploy a simple To-Do application that creates To-Do lists like this:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c18c5105-9464-4bce-b974-47286393f3fb)
# STEP 1 – BACKEND CONFIGURATION
Update ubuntu----sudo apt update
Upgrade ubuntu---sudo apt upgrade
Lets get the location of Node.js software from Ubuntu repositories.-----curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
Install Node.js----sudo apt-get install -y nodejs
Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.
Verify the node and npm installation with the command below----node -v and npm -v 
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/71c3f6ba-2e77-4ef7-92eb-90fd93295823)
Create a new directory for your To-Do project:mkdir Todo
Now change your current directory to the newly created one: cd Todo
Next, you will use the command npm init to initialise your project, so that a new file named package.json will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.-----npm init
# INSTALL EXPRESSJS
Remember that Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of your application based on HTTP methods and URLs.
To use express, install it using npm:----npm install express
Now create a file index.js with the command below-----touch index.js
Install the dotenv module------npm install dotenv
Open the index.js file with the command below-----vim index.js
Type the code below into it and save. 
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});
Now it is time to start our server to see if it works. Open your terminal in the same directory as your index.js file and type:---node index.js

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
Notice that we have specified to use port 5000 in the code. This will be required later when we go on the browser.
Now we need to open this port in EC2 Security Groups. Refer to Project 1 Step 1 – Installing the Nginx Web Server. There we created an inbound rule to open TCP port 80, you need to do the same for port 5000, like this:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/12ee89f1-d011-4729-87eb-7004dd6cd742)
Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000:---------http://<PublicIP-or-PublicDNS>:5000
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c9e6fdee-85dd-4aea-acb9-6860073f6d10)
# Routes
There are three actions that our To-Do application needs to be able to do:
Create a new task
Display list of all tasks
Delete a completed task
Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.
For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes------mkdir routes
Tip: You can open multiple shells in Putty or Linux/Mac to connect to the same EC2
Change directory to routes folder.----cd routes
Now, create a file api.js with the command below-----touch api.js
Open the file with the command below-----vim api.js
Copy below code in the file.
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
# MODELS
Now comes the interesting part, since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model.
A model is at the heart of JavaScript based applications, and it is what makes it interactive.
We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. (Seems like a lot of information, but not to worry, everything will become clear to you over time. I promise!!!)
In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties
To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier.
Change directory back Todo folder with cd .. and install Mongoose-----------npm install mongoose
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/44500659-93fa-46ca-9961-5fd797f39430)
Create a new folder models:------mkdir models
Change directory into the newly created ‘models’ folder with-----cd models
Inside the models folder, create a file and name it todo.js----touch todo.js
Tip: All three commands above can be defined in one line to be executed consequently with help of && operator, like this:----mkdir models && cd models && touch todo.js
Open the file created with vim todo.js then paste the code below in the file:
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
Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.
In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
# MONGODB DATABASE
We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, you will need to sign up for a shared clusters free account, which is ideal for our use case. Sign up here. Follow the sign up process, select AWS as the cloud provider, and choose a region near you.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/7c7552f8-a77b-4b16-85d5-a3351cc028c7)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/34e6af8c-adc7-410e-b660-ee0a1ad83952)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/8126ea98-0600-4f45-8b91-a5d7dbba23c9)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/d8dfa79d-53ca-4ce2-a4c5-ed169cf39380)
In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.
Create a file in your Todo directory and name it .env.------touch .env
vi .env
Add the connection string to access the database in it, just as below: ----(SAMPLE DONT USE IT USE YOUR OWN)DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
Ensure to update <username>, <password>, <network-address> and <database> according to your setup
Here is how to get your connection string
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/ddda5c86-5d99-408d-be0f-4017d9703111)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/c8362724-2fe3-4d92-a4ab-60e0459ff2dc)
Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database.
Simply delete existing content in the file, and update it with the entire code below.-----------Open the file with vim index.js
Now, paste the entire code below in the file.
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
Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

Start your server using the command:

node index.js
You shall see a message ‘Database connected successfully’, if so – we have our backend configured. Now we are going to test it.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/b12ab07a-ff11-4e86-83a4-d70599b1557c)
# STEP 2 – FRONTEND CREATION
Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
In the same root directory as your backend code, which is the Todo directory, run:---npx create-react-app client
This will create a new folder in your Todo directory called client, where you will add all the react code.
# Running a React App
Before testing the react app, there are some dependencies that need to be installed.
Install concurrently. It is used to run more than one command simultaneously from the same terminal window.----npm install concurrently --save-dev
Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.----npm install nodemon --save-dev
In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/95920330-1d51-4fed-ad98-8000f4e7d15f)
# Configure Proxy in package.json
Change directory to ‘client’-------cd client
Open the package.json file----------vi package.json
Add the key value pair in the package.json file -----"proxy": "http://localhost:5000".
Now, ensure you are inside the Todo directory, and simply do:-----npm run dev
Your app should open and start running on localhost:3000
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/6cd7383d-ff78-45db-9990-41ec3f760346)
# Important note: 
In order to be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule. You already know how to do it.
Creating your React Components
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
From your Todo directory run---------------cd client
move to the src directory--------cd src
Inside your src folder create another folder called components--------mkdir components
Move into the components directory with---------cd components
Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.---------touch Input.js ListTodo.js Todo.js
Open Input.js file-------vi Input.js
Copy and paste the following
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
To make use of Axios, which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.
Move to the src folder---cd ..
Move to clients folder---------cd ..
Install Axios---------npm install axios
# FRONTEND CREATION (CONTINUED)
Go to ‘components’ directory---cd src/components
After that open your ListTodo.js----vi ListTodo.js
in the ListTodo.js copy and paste the following code
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
Then in your Todo.js file you write the following code

import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.
Move to the src folder------------cd ..
Make sure that you are in the src folder and run------vi App.js
Copy and paste the code below into it
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
After pasting, exit the editor.

In the src directory open the App.css

vi App.css
Then paste the following code into App.css:

.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
Exit
In the src directory open the index.css-------vim index.css
Copy and paste the code below:
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
Go to the Todo directory

cd ../..
When you are in the Todo directory run:------npm run dev
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/02209602-d7cb-4dc0-b723-5e5da59fe931)









