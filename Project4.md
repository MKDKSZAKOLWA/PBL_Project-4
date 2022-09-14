# (STEP 10) PROJECT 4 MEAN STACK DEPLOYMENT TO UBUNTU IN AWS 


## Task



In this assignment you are going to implement a simple Book Register web form using MEAN stack.


## Step 1: Install NodeJs


Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.


Update ubuntu


`sudo apt update`


![Update Ubuntu](./Images/Update%20Ubuntu.PNG)


Upgrade ubuntu


`sudo apt upgrade`


![Upgrade Ubuntu Screen1](./Images/Upgrate%20Ubuntu%20Screen1.PNG)



When asked , "Do you want to continue?". Select Y


![Upgrade Ubuntu Screen2](./Images/Upgrate%20Ubuntu%20Screen2.PNG)



Add certificates


`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`


![Add Certificates 1](./Images/Add%20Certificates%201.PNG)



`curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`

![Add Certificates 2](./Images/Add%20Certificates%202.PNG)



Install NodeJS


`sudo apt install -y nodejs`


![Install NodeJS](./Images/Install%20NodeJS.PNG)


To confirm the version installed, run the command below.


`node --version`


![Confirm Node Version](./Images/Confirm%20Node%20Version.PNG)


As shown above, the repository version of nodeJS is an older version (12.x) A newer one needs to be installed. The latest version is 18.x at this time. Edit the setup to reflect the latest version of nodeJS. 


`curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -`


![Add Certificates_3](./Images/Add%20Certificates_3.PNG)

![Add Certificates_4](./Images/Add%20Certificates_4.PNG)


Run the command below to install a new version of nodeJS.


` sudo apt install -y nodejs`


![NodeJS New Version](./Images/NodeJS%20New%20Version.PNG)




You can confirm the installation of the newer version of nodeJS by running the command below.

`node -v`


![NodeJS Version 18](./Images/NodeJS%20Version%2018.PNG)




## Step 2: Install MongoDB



MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
mages/WebConsole.gif



First add MongoDB key server by running the command below.


`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`



![Add MongoDB Key Server](./Images/Add%20MongoDB%20Key%20Server.PNG)



Run the below command to add MongoDB repository.



`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`



![Add MongoDB Repository](./Images/Add%20MongoDB%20Repository.PNG)



Install MongoDB


`sudo apt install -y mongodb`


![Install MongoDB](./Images/Install%20MongoDB.PNG)


Start The server.


`sudo service mongodb start`


![Start Server](./Images/Start%20Server.PNG)


Verify that the service is up and running.


`sudo systemctl status mongodb`


![Verify Service Running](./Images/Verify%20Service%20Running.PNG)


No need to run the command to install npm as it was already installed by installing NodeJs. You can run the command below to confirm the version of npm.


`![Confirm npm version](./Images/Confirm%20npm%20Version.PNG)


Install body-parser package.


We need ‘body-parser’ package to help us process JSON files passed in requests to the server.


`sudo npm install body-parser`


![Install Body Parser](./Images/Install%20Body%20parser.PNG)


It shows that there is a newer version for Body Parser that you can install by the command below.

`sudo npm install -g npm@8.19.1`


![Latest npm Version](./Images/Latest%20npm%20Version.PNG)


Confirm the installation of Latest npm version .

`npm -v`

![Confirm npm Latest Version](./Images/Confirm%20npm%20Latest%20Version.PNG)



Create a folder named ‘Books’.


`mkdir Books && cd Books`


![Create Books Folder](./Images/Create%20Books%20Folder.PNG)



In the Books directory, Initialize npm project


`npm init`


* Press "Enter" after package name :(books)
* Press "Enter" after version: )1.0.0)
* Under description, you can put anything. In our case we have A books register app, then "Enter".
* Put server.js under entry point then "Enter".
* Leave test command blank and then click "Enter".
* Leave git repository blank and then click "Enter".
* Leave keywords blank and then click "Enter".
* Put any name under author and then "Enter".
* Put anything under license and then "Enter".



![npm init screen 1](./Images/npm%20init%20screen%201.PNG)


* Click "Enter" at the bottom when asked "Is this ok? (yes).


![npm init screen 2](./Images/npm%20init%20screen%202.PNG)

Run `ls` 

![ls for books](./Images/ls%20for%20books.PNG)


Add a file to it named server.js


`vi server.js`


Copy and paste the web server code below into the server.js file.



```
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
```
When the text editor opens up, press i to Insert.
Copy and paste the code in the text editor.


![Paste Code 1](./Images/Paste%20Code%201.PNG)



Use :w to save in vi and use :qa to exit vi.
To do this, you need to press esc then shift + : then type w and enter. 

![Save Server.js file](./Images/Save%20Server.js%20file.PNG)


Then shift + : then qa and enter.


![Quit Server.js file](./Images/Quit%20Server.js%20file.PNG)


Confirm the content of the file you have just created.

`cat server.js`


![Confirm Server.js contents](./Images/Confirm%20Server.js%20contents.PNG)


## INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER.


## Step 3: Install Express and set up routes to the server
*Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.*

*We also will use Mongoose package which provides a straight-forward, schema-based solution to model your application data. We will use Mongoose to establish a schema for the database to store data of our book register.*


`sudo npm install express mongoose`


![Install Express Mongoose](./Images/Install%20Express%20Mongoose.PNG)



In ‘Books’ folder, create a folder named apps.


`mkdir apps && cd apps`


![Create apps folder](./Images/Create%20apps%20folder.PNG)


Run `cd ..` to go back to Books folder.


![Directory Change Books](./Images/Directory%20Change%20to%20Books.PNG)



Run `ls` to confirm apps folder.


![Confirm apps folder](./Images/Confirm%20apps%20folder.PNG)


Run `cd apps` to go back into apps directory.


![Directory Change apps](./Images/Directory%20Change%20apps.PNG)



Create a file named routes.js.


`vi routes.js`


Copy and paste the code below into routes.js.



```
var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
```

When the text editor opens up, press i to Insert.

Copy and paste the code below into routes.js


![Paste Code 2](./Images/Paste%20Code%202.PNG)


Use :w to save in vi and use :qa to exit vi.
To do this, you need to press esc then shift + : then type w and enter. 


![Save routes.js](./Images/Save%20routes.js)


Then shift + : then qa and enter.


![Quit routes.js](./Images/Quit%20routes.js)



The screen below is diplayed.


![Quit routes.js page2](./Images/Quit%20routes.js%20page2.PNG)



Run `cat routes.js` top confirm that the file is saved.


![Confirm routes.js](./Images/Confirm%20routes.js)


In the ‘apps’ folder, create a folder named models.


`mkdir models && cd models`


![Create models](./Images/Create%20models.PNG)


Create a file named book.js.


`vi book.js`


When the text editor opens up, press i to Insert.

Copy and paste the code below into book.js


```
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
```


![Paste Code 3](./Images/Paste%20Code%203.PNG)


Use :w to save in vi and use :qa to exit vi.
To do this, you need to press esc then shift + : then type w and enter. 


![Save book.js](./Images/Save%20book.js)



Then shift + : then qa and enter.


![Quit book.js](./Images/Save%20book.js)


The screen below will be displayed.

![Quit book.js Page2](./Images/Quit%20book.js%20Page2.PNG)


Run `cat book.js` to confirm the file was saved.



![Confirm book.js Saved](./Images/Confirm%20book.js%20Saved.PNG)



## Step 4 – Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’


`cd ../..`


![Directory Change Books2](./Images/Directory%20Change%20Books2.PNG)


Create a folder named public and cd into it.


`mkdir public && cd public`


![Create public folder](./Images/Create%20public%20folder.PNG)


Add a file named script.js.


`vi script.js`


When the text editor opens up, press i to Insert.

Copy and paste the Code below (controller configuration defined) into the script.js file.



```
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
```


![Paste Code 4](./Images/Paste%20Code%204.PNG)



Use :w to save in vi and use :qa to exit vi.
To do this, you need to press esc then shift + : then type w and enter. 


![Save Controller Configuration](./Images/Save%20Controller%20Configuration.PNG)



Then shift + : then qa and enter.


![Quit Script.js](./Images/Quit%20Script.js)


The screen below will be displayed.

![Quit Script.js 2](./Images/Quit%20Script.js%202.PNG)


In public folder, create a file named index.html;


`vi index.html`


When the text editor opens up, press i to Insert.


Copy and paste the code below into index.html file.


```
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
```


![Paste Code 5](./Images/Paste%20Code%205.PNG)


Use :w to save in vi and use :qa to exit vi.
To do this, you need to press esc then shift + : then type w and enter.


![Save index.html](./Images/Save%20index.html)



Then shift + : then qa and enter.


![Quit index.html](./Images/Quit%20index%2Chtml.PNG)


The  screen below will be displayed.


![Index.html 2](./Images/Quit%20index%2Chtml%202.PNG)



Run `cat index.html` to confirm that the file is saved.



![Confirm index.html](./Images/Confirm%20index.html%20Saved.PNG)


Change the directory back up to Books.


`cd ..`


![Directory Change Books 3](./Images/Directory%20Change%20Books%203.PNG)




Start the server by running this command:


`node server.js`


![Start Server 2](./Images/Start%20Server%202.PNG)



The server is now up and running, we can connect it via port 3300. You can launch a separate Putty or SSH console to test what curl command returns locally.

`curl -s http://localhost:3300`


To do this open GIT bash app. The scren below will be displayed.


![Open GIT Bash App](./Images/Open%20GIT%20Bash%20App.PNG)

Run `cd downloads`


![CD downloads](./Images/CD%20downloads.PNG)



To connect using SSH, click the SSH tab in your EC2. Copy the link under Example.


![SSH EC2](./Images/SSH%20EC2.PNG)


Paste the SSH link to GIT Bash and enter.


![Paste SSH Git Bash](./Images/Paste%20SSH%20Git%20Bash.PNG)



Now the command below .

`curl -s http://localhost:3300`


![Curl localhost](./Images/Curl%20localhost.PNG)


*It shall return an HTML page, it is hardly readable in the CLI, but we can also try and access it from the Internet.*

*For this – you need to open TCP port 3300 in your AWS Web Console for your EC2 Instance.*

Go to the EC2 Instance.

Check mark the instance that is running or you are using and click on the security tab and then click on security groups as shown below.


![Open Port Step 1](./Images/Open%20Port%20Step%201.PNG)


Under inbound rules, click on Edit inbound rules.


![Open Port Step 2](./Images/Open%20Port%20Step%202.PNG)


Click Add rule.


![Open Port Step 3](./Images/Open%20Port%20Step%203.PNG)

Fill as shown on the screen below and Save.

* Type : Custom TCP
* Port Range : 3300
* Source : Anywhere


![Open Port Step 4](./Images/Open%20Port%20Step%204.PNG)


*Your Sercurity group shall look like this:*


![Open Port Step 5](./Images/Open%20Port%20Step%205.PNG)


Now we can access our Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

1. You can find it in your AWS web console in EC2 details.

2. Run `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address or
 `curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name.


The  current Public IP address is 18.222.14.130


Run the IP and the port in the web as shown below.


18.222.14.130:3300


This is how your Web Book Register Application will look like in browser:


![Web Book Register Application](./Images/Web%20Book%20Register%20Application.PNG)


Project Completed.