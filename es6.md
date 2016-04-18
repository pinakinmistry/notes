Object Enhancements:
====================

- Short hand notation:
var var1 = "var1";
var var2 = "var2";
function func() {};

var obj1 = {
	var1,
	var2,
	func
};

var obj2 = {
	var1,
	var2,
	func() {}
};

var obj3 = {
	var1,
	var2,
	["func"]: function () {}
};

var funcName = "func";

var obj4 = {
	var1,
	var2,
	[funcName]			//Computed property
};

- Spread operator:
var a = [1, 2, 3];
b = [4, 5, 6];

a.push(b); //[1, 2, 3, [4, 5, 6]]  -> Undesired
a.push(...b); //[1, 2, 3, 4, 5, 6]  -> As desired










@ Frontendmasters:
==================

Tail Position:
- return expression
- f() { return tail_position; }

Tail Call:
- calling a function at tail position
- f() { return y(); }

Close Call:
- return expression with tail call(s)
- f() { return 1 + y(); }

Proper Tail Call (PTC):
- New to ES6
- works only in strict mode
- No stacking up and going out of memory
- Previous stacks are GC'ed
- Optimize call so that tail position is tail call
- f() { return f(); }


Example of running out of memory:

Ex1:
f = (num) => {
	try {
		return f((num || 0) + 1);
	}
	catch(e) {
		return num;
	}
}

console.log(f());

Ex2:
fibonacci = (x, y, limit, index) => {
	if(arguments.length == 1) {
		if(x) {
			return fibonacci(0, 1, x, 1);
		}
		return 0;
	} else {
		if(index < limit) {
			return fibonacci(y, (x + y), limit, ++index);
		}
		return y;
	}
}

console.log(fibonacci(3)); //2
console.log(fibonacci(10)); //55
console.log(fibonacci(12000)); //Range error


ES6 Support Table:
==================

https://kangax.github.io/compat-table/es6/


CONST, LET and Blocks:
======================
let:
- local variables within a block, for, while statement
- lexically scoped

const:
- defines constant
- lexically scoped

block:
- {} gives rise to new lexical scope


Rest Parameters:
================
- ...parameter as last argument in the function definition
- eliminates the need of arguments object
- only one rest parameter in a function
- cant't use arguments
- no default value with rest parameter

Temporal dead zone ? Area of code wherein the variable is not declared yet so we will be dead if we try to use it in this zone.


Spread Operator:
================
- breaks array into pieces


Destructing:
============
- {} on LHS of =
- {a, b, c} = getABC();
- can give alias
- {A: a, B: b, C: c} = getABC(); //a, b, c exists as alias of A, B, C returned by getABC()
	- destucturing followed by assignment
- even in function parameter list: f({a, b}) //destructure a and b properties from incoming object
- Irrefutable, unforgiving as in properties asked in destructing must be there in the incoming object
- Change it to forgiving/refuting by prefixing ? to the property. {a, ?b} = object; //it's ok if b not thre in object
- let ?{a, b, c} = object
- ?{a} works even if incoming is not an object. {?a} doesn't.
- nested destructing
	- let {name, address: {street, city, zip}} = person; //we wont get address variable
- [one, two,,,five] = [1,2,3,4,5]



Default Parameters:
=========
f({a = 'A', b = 'B'}) {
	//A and B are default values if not passed
}

- can invoke a function to get default at runtime
f (a = func());

- undefined value triggers default parameter assigment. It's triggered only once so that it doesn't loop forever if even default is undefined.

- default parameters are not counted in arguments object.



Arrow function:
===============

- square = x => x * x;
- square = (x) => x * x; //() optional when one parameter only
- they eliminate the need of self = this with help of lexical binding of this
- can't use new() on array function to use it as constructor (bcos this is always lexically bound)
- can't alter this on arrow function. can't use bind, call, apply
- this is always bound to window when it evaluates the arrow function
- can't use it for methods inside object along with this


Classes:
========
- class is just sugar; function Bar and class Bar are functionally same.
	function Bar() {

	}

	class {

	}

- Syntax:

	class Monster {
		var monsterHealth = Symbol();
		constructor(name, health) {
			this.name = name;
			this[monsterHealth] = health;
		}

		get isAlive() {
			return this[monsterHealth] > 0;
		}

		set isAlive(alive) {
			if(!alive) {
				this[monsterHealth] = 0;
			}
		}

		checkAlive() {
			return this.isAlive;
		}
	}

	var kevin = new Monster('Kevin', 100);
	kevin.checkAlive();
	kevin.isAlive = false;
	kevin.checkAlive();

- class property:
	Monster.allMonsters = [];

- extend class:
	class Godzilla extends Monster {
		constructor(...) {
			super('Godzilla', 100000);
		}
	}


	extends expression can be a method call which returns a class that will be extended.

- default constructor:
	constructor(...args) {
		super(...args);
	}

Set, Map, Weak Map:
===================
- push()
- get()
- has()
- clear()
- delete()
- size

- set()
- get()
- size

- set()
- get()

Modules:
========

- bits of both CommonJS and AMD
- Syntax:
	Single/default export:
	//MyClass.js
	class MyClass {}
	export default MyClass;
	
	//main.js
	import MyClass from 'MyClass';


	Multi export:
	//lib.js
	export const sqrt = Math.sqrt;
	
	export function square(x) {
		return x * x;
	}

	export function diag(x, y) {
		return sqrt(square(x) + square(y));
	}
	
	//main.js
	import {square, diag} from 'lib';
	square(10);
	diag(10, 20);
	or 
	import * as lib from 'lib';
	lib.square(10);

	Alias:
	class MyClass {

	}
	export {MyClass as Dude};

	import {Dude as Bro} from 'lib';
	var bro = new Bro();

- Async loading:
	System.import('<path>')
	.then(module_name => {

	})
	.error(error =. {

	});


	Promise.all(
		['module1', 'module2', 'module3'].map(module => System.import(module)))
	.then(function([module1, module2, module3]) {

	})

Promises:
=========

- Syntax:
	var promise = new Promise(function(resolve, reject) {
		if(allGood) 
			resolve(data);
		else
			reject(new Error('something went wrong'));	
	});

	return promise;

	promise.then(function(result) {
		console.log(result);
	}, function(error) {
		console.log(error);
	});

	or

	promise.then(function (result) {})
	.catch(function (error) {});


- Promise.all([promise1, promise2])
	.then(function(results) {
		//results[0] is result from promise 1 and results[1] is result from promise 2
	})

- chain then to chain the aysnc result:
	

Sachin: 8425040305 R6