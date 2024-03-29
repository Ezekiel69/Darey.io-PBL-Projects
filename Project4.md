# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS
Now, when you have already learned how to deploy LAMP, LEMP and MERN Web stacks – it is time to get yourself familiar with MEAN stack and deploy it to Ubuntu server.
# MEAN Stack is a combination of following components:
MongoDB (Document database) – Stores and allows to retrieve data.
Express (Back-end application framework) – Makes requests to Database for Reads and Writes.
Angular (Front-end application framework) – Handles Client and Server Requests
Node.js (JavaScript runtime environment) – Accepts requests and displays results to end user
In order to complete this project you will need an AWS account and a virtual server with Ubuntu Server OS.
If you do not have an AWS account – go back to Project 1 Step 0 to sign in to AWS free tier account ans create a new EC2 Instance of t2.nano family with Ubuntu Server 22.04 LTS (HVM) image. Remember, you can have multiple EC2 instances, but make sure you STOP the ones you are not working with at the moment to save available free hours.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/87362c64-00aa-4e54-be0c-623352c18105)
# Hint: 
In previous projects we used different tools to connect to an EC2 instance, but if you do not want to install or launch anything outside of AWS, you can open youc CLI straight from Web Console in AWS, like this:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/ea304d01-cd13-4ed7-abaa-0aa9ec4188a7)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/4332e914-1758-4cf8-b3e5-59a18f1536c0)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/4b75ca73-4874-4b23-a3d7-83e680bab34f)
# Task
In this assignment you are going to implement a simple Book Register web form using MEAN stack.
# Step 1: Install NodeJs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.
Update ubuntu--------------sudo apt update
Upgrade ubuntu-------------sudo apt upgrade
Add certificates----sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates--------curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
Install NodeJS--------sudo apt install -y nodejs
# Step 2: Install MongoDB
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
sudo apt-get install gnupg curl curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \    
sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \ --dearmor 
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list 
sudo apt-get update -------------------------------------------------If you are using ubuntu 22.04
Install MongoDB------------sudo apt-get install -y mongodb-org 
Start The server-----------sudo service mongodb start
Verify that the service is up and running-----------sudo systemctl status mongodb
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/723b24f1-7c1c-4979-831d-3ed302b559c8)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/e4663ba1-dffb-4186-a15d-d4c5476f4e76)
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/a01d8127-9765-4782-94c9-5e0a2e198fe6)
Install npm – Node package manager.-----------------sudo apt install -y npm
Install body-parser package
We need ‘body-parser’ package to help us process JSON files passed in requests to the server.---------sudo npm install body-parser
Create a folder named ‘Books’----------------mkdir Books && cd Books
In the Books directory, Initialize npm project--------------------npm init
Add a file to it named server.js----------------------vi server.js
Copy and paste the web server code below into the server.js file.
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/f52c1730-2858-4961-9170-44adfaca67a4)
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
# INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER
# Step 3: Install Express and set up routes to the server
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.
We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.
sudo npm install express mongoose
In ‘Books’ folder, create a folder named apps-------------------mkdir apps && cd apps
Create a file named routes.js-----------------------------------vi routes.js
Copy and paste the code below into routes.js
const Book = require('./models/book');

module.exports = function(app){
  app.get('/book', function(req, res){
    Book.find({}).then(result => {
      res.json(result);
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while retrieving books');
    });
  });

  app.post('/book', function(req, res){
    const book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save().then(result => {
      res.json({
        message: "Successfully added book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while saving the book');
    });
  });

  app.delete("/book/:isbn", function(req, res){
    Book.findOneAndRemove(req.query).then(result => {
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while deleting the book');
    });
  });

  const path = require('path');
  app.get('*', function(req, res){
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
  });
};
In the ‘apps’ folder, create a folder named models------------mkdir models && cd models
Create a file named book.js-----------------------------------vi book.js
Copy and paste the code below into ‘book.js’
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
# Step 4 – Access the routes with AngularJS
AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.
Change the directory back to ‘Books’--------------cd ../..
Create a folder named public----------------------mkdir public && cd public
Add a file named script.js------------------------vi script.js
Copy and paste the Code below (controller configuration defined) into the script.js file.
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
In public folder, create a file named index.html;-----------vi index.html
Cpoy and paste the code below into index.html file.
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
Change the directory back up to Books----------------------------cd ..
Start the server by running this command:-------------------------node server.js
The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.
curl -s http://localhost:3300
It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.
For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.
You are supposed to know how to do it, if you have forgotten – refer to Project 1 (Step 1 — Installing Apache and Updating the Firewall)
Your Security group shall look like this:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/649c2585-3078-4023-b30d-d50f1de6ac2d)
Now you can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.
Quick reminder how to get your server’s Public IP and public DNS name:
1. You can find it in your AWS web console in EC2 details
2. Run curl -s http://169.254.169.254/latest/meta-data/public-ipv4 for Public IP address or curl -s http://169.254.169.254/latest/meta-data/public-hostname for Public DNS name.
This is how your Web Book Register Application will look like in browser:
![image](https://github.com/Ezekiel69/Darey.io-PBL-Projects/assets/66247894/7ec3ea15-fb0a-48bf-8243-a76281256eb0)
Congratulations!
You have now completed all ‘PBL Progressive’ projects and are ready to move on to more complex and fun ‘PBL Professional’ projects!!!
