#Node API Design (FEM):

//Source: http://fem-node-api.netlify.com/

All nodejs files are wrapped inside this IIFE:
(function (module, exports, __dirname) {
	<nodejs file content goes here>
})();

## exports vs module.exports

exports is object that we can attached different pieces of code
module.exports is the entire thing that we want to export

exports.func1 = function () {};
exports.func2 = function () {};
//it's always an object when ask for it in other nodejs file using require function

or

module.exports = {
	func1: function () {},
	func2: function () {}
};
//now it can't export anything else
//it can be primitive as well.

exports = module.exports = {};
//safegaurds that they both are same and nothing else get exported later on.

## Creating a server using express:

express is routing and middleware framework.

var express = require('express');
var app = express();
var fs = require('fs');

app.get('/', function(req, res) {
	//using fs
	fs.readFile('index.html', function (err, buffer) {
		var html = buffer.toString();
		res.setHeader('Content-Type', 'text/html');
		res.send(html);
	});
	//or using sendFile()
	res.sendFile(__dirname, '/index.html', function (err) {
		if(err) {
			res.status(500).send(err);
		}
	});
});

var port = 3000;
app.listen(port, function() {
	console.log('listening to port', port);
});

### methods and props on res and req objects:
res.sendFile()
res.send()
res.json()
res.status()

req.body.postedData;
req.params.id;

## Getting RESTful:

- stateless (Any request shouldn't be dependent on previous requests)
- uses http verbs explicitly
- directory like url pattern for routes/resources
- transfer JSON/XML

## Exercise 2:

var express = require('express');
var bodyParser = require('body-parser');
var _ = require('lodash');
var app = express();

app.use(express.static('client'));
app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());

var lions = [];

app.get('/lions', function (req, res) {
	res.json(lions);
});

app.get('/lions/:id', function (req, res) {
	var lion = _.find(lions, { id: req.params.id });
});

app.post('/lions', function (req, res) {
	var lion = req.body;
	id++;
	lion.id = id + '';
	lions.push(lion);
	res.json(lion);
});

app.put('/lions/:id', function (req, res) {
	var update = req.body;
	if(update.id) {
		delete update.id;
	}
	var index = _.findIndex(lions, { id: req.params.id });
	if(!lions[index]) {
		res.send();
	} else {
		var updatedLion = _.assign(lions[index], update);
		res.json(updatedLion);
	}
});

app.delete('/lions/:id', function (req, res) {
	var index = _.findIndex({ id: req.params.id });
	if(!lions[index]) {
		res.send();
	} else {
		var deletedLion = lions[index];
		lions.splice(index, 1);
		res.send(deletedLions);
	}
});

## Middleware:

Middleware is a function with access to request object, response object and the next function that when called will go to next middleware in the stack.

app.use(<middleware>);

### Types of middleware:

5 types in express 4.x:
- 3rd party
- router level
- application level
- error handling
- built in

#### error handler:

//error handler always has 4 parameters
app.use(function(err, req, res, next) {
	console.log('Error:', err);
});


#### built in:

app.use(express.static('client'));

app.param('id', function (req, res, next, id) {
	var lion = _.find(lions, { id: id });
	if(lion) {
		req.lion = lion;
		next();
	} else {
		res.send();
	}
});

//Now use req.lion directly in get, put and delete handlers

#### router level:

//update id whenever post is called

var updateId = function (req, res, next) {
	if(!req.body.id) {
		id++;
		req.body.id = id + '';
	}
	next();
}

app.post('/lions', updateId, functiom(req, res) {
	var newLion = req.body;
	...
});


## Routers:

With express 4, we can have more than one router in our application. Think of router as a module with its own routing and middleware stack and functionality. This allows more fine grain control over our resources. Great for versioning of apis.

var express = require('express');
var app = express();
var todoRouter = express.Router();

todoRouter.get('/', function (req, res) {
	res.json(todos);
});

app.use('/todos', todoRouter); //mounts the router to the given url

For eg:
//server.js
var lionRouter = require('./lions');
app.use('/lions', lionRouter);

//lions.js:
var lionRouter = require('express').Router();
...
module.exports = lionRouter;


### concat handlers using route method:

lionRouter.route('/')
	.get(function (req, res) {
		res.json(lions);
	})
	.post(updateId, function (req, res) {
		var tiger = req.body;
		tiger.push(tiger);
		res.json(tiger);
	});


lionRouter.route('/:id')
	.get(function (req, res) {
		...
	})
	.put(function (req, res) {
		...
	})
	.delete(function () {
		...
	});

## Testing:
Using supertest module

//server.js
var app = ...
...
module.exports = app;

//serverTest.js
var app = require('./app');
var request = require('superset');

describe('todos', function () {
	it('should get all todos', function (done) {
		request(app)
			.get('/todos')
			.set('Accept', 'application/json')
			.expect(200)
			.end(function (err, resp) {
				expect(resp.id).toBeDefined();
				done();
			});
	});
});

## Node environment variables:

The environment variables exists in process.env variable.

process.env.NODE_ENV = testing | development | production

//config.js
var config = {
	env: process.env.NODE_ENV || 'development',
	logging: false,
	secret: {
		githubToken: process.env.GITHUB_TOKEN,
		jwtToken: process.env.JWT_TOKEN
	}
};

var envConfig = require('./' + config.env);
module.exports = _.assign(config, envConfig || {});


## Mongo and mongoose:

mongod
mongo

use test //db name
db.getCollectionNames()
db.<collection>.insert({...})


var mongoose = require('mongoose');

var TodoSchema = new mongoose.Schema({
	completed: Boolean,
	content: {
		type: String,
		required: true
	}
});

var TodoModel = mongoose.model(<lowercase collection name>, TodoSchema);
TodoModel.create({
  name: 'clean up your room!!!',
  completed: false
}).then(function(err, todo) {
  console.log(err, todo);
});


## Data modeling using Schema:

## Querying:

### population:

## Promises:

function action() {
	return new Promise(function (resolve, reject) {
		setTimeout(function () {
			resolve('here i come');
		}, 5000);
	});
}

action()
	.then(function (response) {
		console.log(response);
	});

//ex2:

var fs = require('fs');

var readFile = function () {
	return Promise(function (resolve, reject) {
		fs.readFile('./package.json', function (err, file) {
			return err ? reject(err) : resolve(file.toString());
		});
	});
};

readFile()
	.then(function (file) {
	})
	.catch(function (err) {
	});

//ex3:

var readAllFiles = function (files) {
	return Promise.all(files)
		.then(function (files) {
			return files;
		});
}


var files = [readFile(), readFile(), readFile()];

readAllFiles(files)
	.then(function () {
		console.log(files);
	});


## JSON Web Token (JWT):

var user = {id: '124352643'};

var token = jwt.sign(user, <secret key>);

//get user back
var user = jwt.verify(req.headers.authorization, <secret key>);

//check whether this user still exists
User.findById(user._id, function () {});

//Add methods to Schema:
TodoSchema.methods.func = function () {} //available at todo instance level
TodoSchema.statics.funct = function () {} //avaiable at Todo collection level


## Deployment:

create procfile:
web: node index.js

npm i --production

npm shrinkwrap //freeze all the versions of dependencies

create new app on heroku

heroku login

git add remote heroku

git co master

git rebase orgin/step-12-fix

git push heroku master //builds stuff

heroku open

heroku logs --tail

foreman start //local testing on port 5000


## Performance

node pm2
node forever
node-inspector ...



## Exercises:

Exercise 4: get /tigers/1 giving 500
	use route method
Exercise 5: Unit testing
Exercise 6: Blogging app with app organization and config
Exercise 7: Mongo, mongoose and schema - DONE
Exercise 8: create schemas and models - DONE
Exercise 9: create CRUD methods in the controllers - DONE
Exercise 10: use controllers in routers (createRoutes in util) - DONE
Exercise 11: implementing authentication using jwt - DONE
Exercise 12: securing apis and creating /me api - DONE
