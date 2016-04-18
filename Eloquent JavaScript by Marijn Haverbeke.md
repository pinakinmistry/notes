# Eloquent JavaScript by Marijn Haverbeke




2. Program Structure:

- Any piece of code that produces value is called an expression.




3. Functions:

- Function wraps a logical block of code. It is useful to structure larger programs, improve readability and reduce repetition.

- Function anatomy:
	function functionName(parameters) {
		//body ...

		//return ...	
	}

	functionName(parameters) //function call

- Scope:
	- Variables defined inside function body are local. Anything defined outside are global.
	- parameters are local to function.
	- Functions can be created inside other functions, producing nested scope/localities.
		function A() {
			var a = 'a';
			B();
			function B() {
				var b = a + 'b';
				C();
				function C() {
					var c = b + 'c';
					console.log(c);
				}
				//console.log(c);
			}
			console.log(b);
		};
	
		This is called lexical scoping.
	- Variables defined in any simple {} block, loops or conditional blocks are not local to that block.
	- Only functions give rise to new scope.

- Functions are first class citizens:
	var add = function (a, b) {
		return a + b;
	}

	add(1, 2);
	add.call(null, 1, 2);
	add.apply(null, [1, 2]);

	Function being an object as properties:
	- name
	- arguments
	- See add. in console

- Functions as values:
	- Function names acts as variables holding the piece of code defined in it.
		function test() { console.log('testing'); }
		test();
		test = 1;
		test();
		console.log(test);

	- Function Declaration vs Function Definition
	- Function hoisting


- Closure:
	(function () {
			var a = 'a';
			return function () {
				var b = a + 'b';
				return b;
			}
		})()();


	(function () {
		var a = 'a';
		return function () {
			var b = a + 'b';
			return function () {
				var c = b + 'c';
				return c;
			};
		};
	})()()();

	- think of Closure as returning a handle to a piece of computation, frozen for later use.


4. Objects and Arrays:





5. Higher-Order Functions

- A large program is a costly program.
- Large size brings in complexity and confuses programmers and invites bugs that are hard to find.
	var total = 0, count = 1; while (count <= 10) {
	total += count;
	count += 1; }
	console.log(total);

	console.log(sum(range(1, 10)));
- sum and range are expressing simpler concepts than the program as a whole, they are easier to get right.
- This is called abstraction.
- Abstractions hide details and give us the ability to talk about problems at a higher (or more abstract) level.
- As an analogy, compare these two recipes for pea soup:
	Put 1 cup of dried peas per person into a container. Add water until the peas are well covered. Leave the peas in water for at least 12 hours. Take the peas out of the water and put them in a cooking pan. Add 4 cups of water per person. Cover the pan and keep the peas simmering for two hours. Take half an onion per person. Cut it into pieces with a knife. Add it to the peas. Take a stalk of celery per person. Cut it into pieces with a knife. Add it to the peas. Take a carrot per person. Cut it into pieces. With a knife! Add it to the peas. Cook for 10 more minutes.
- And the second recipe:
	Per person: 1 cup dried split peas, half a chopped onion, a stalk of celery, and a carrot.
	Soak peas for 12 hours. Simmer for 2 hours in 4 cups of water (per person). Chop and add vegetables. Cook for 10 more minutes.

- Abstracting array traversal:
	var array = [1, 2, 3];
	for (var i = 0; i < array.length; i++) {
		var current = array[i];
	console.log(current); }

	function logEach(array) {
	for (var i = 0; i < array.length; i++)
	console.log(array[i]); }

- What if you do something other than logging:
	function forEach(array , action) {
		for (var i = 0; i < array.length; i++)
			action(array[i]);
	}

	forEach(["Wampeter", "Foma", "Granfalloon"], console.log);

- Using built in forEach instead of for loop:
	function nestedForLoops(books) {
		var booksObj = {};
		for (var book = 0; book < books.length; book++) {
			var chapters = books[book].chapters;
			booksObj[books] = {}
			for (var i = 0; i < chapters.length; i++) {
				var chapter = chapters[i];
				booksObj[books][chapter] = chapter.tableOfContent;
			}
		}
		return booksObj;
	}

	//Using forEach loop:
	function nestedForLoops(books) {
		var booksObj = {};
		books.forEach(function(book) {
			booksObj[books] = {}
			book.chapters.forEach(function(chapter) {
				booksObj[books][chapter] = chapter.tableOfContent;
			});
		});
		return booksObj;
	}

Higher-order functions:
- Functions that operate on other functions, either by taking them as arguments or by returning them, are called higher-order functions.

	function greaterThan(n) {
		return function(m) { return m > n; };
	}
	var greaterThan10 = greaterThan(10); console.log(greaterThan10(11)); // → true

	function noisy(f) {
		return function(arg) {
			console.log("calling with", arg);
			var val = f(arg);
			console.log("called with", arg, "- got", val); return val;
		};
	}

	noisy(Boolean)(0);
	// → calling with 0
	// → called with 0 - got false

Passing along arguments:
- What if there are more than one argument ?

	function noisy(f) {
		return function () {
			return f.apply(null, arguments);
		};
	}

Filtering an array:
	function filter(array , test) {
		var passed = [];
		for (var i = 0; i < array.length; i++) {
			if (test(array[i]))
				passed.push(array[i]);
		}
		return passed; 
	}


	console.log(filter(books, function(book)
		return book.pages > 500;
	}));

	console.log(books.filter(function(book)
		return book.pages > 500;
	}));


Transforming with map:
	function map(array , transform) {
		var mapped = [];
		for (var i = 0; i < array.length; i++)
		mapped.push(transform(array[i]));
		return mapped;
	}

	console.log(map(books, function(book) {
		return book.name;
	}));

Summarizing with reduce (folding):
	
	function reduce(array , combine , start) {
		var current = start;
		for (var i = 0; i < array.length; i++)
			current = combine(current , array[i]);
		return current;
	}

	console.log(reduce([1, 2, 3, 4], function(a, b) { return a + b; }, 0)); // → 10

	console.log(ancestry.reduce(function(min, cur) {
		if (cur.born < min.born)
			return cur;
		else return min;
	}));

Composability:
	
	function average(array) {
		function plus(a, b) { return a + b; }
		return array.reduce(plus) / array.length;
	}

	function age(p) { return p.died - p.born; }
	function male(p) { return p.sex == "m"; }
	function female(p) { return p.sex == "f"; }

	console.log(average(ancestry.filter(male).map(age)));
	console.log(average(ancestry.filter(female).map(age)));


Finding % DNA shared with an ancestor:
- We share more than 1/2^G of my genes with an ancestor, where G is number of generations between the ancestor and us. This formula comes from the idea that each generation splits the gene pool in two.

	var byName = {};
	ancestry.forEach(function(person) {
		byName[person.name] = person;
	});

	console.log(byName["Philibert Haverbeke"]); // → {name: "Philibert Haverbeke", ...}

	function reduceAncestors(person , f, defaultValue) {
		function valueFor(person) {
			if (person == null)
				return defaultValue;
			else
				return f(person, valueFor(byName[person.mother]), valueFor(byName[person.father]));
		}
		return valueFor(person);
	}

	function sharedDNA(person , fromMother , fromFather) {
		if (person.name == "Pauwels van Haverbeke")
			return 1;
		else
			return (fromMother + fromFather) / 2;
	}

	var ph = byName["Philibert Haverbeke"];
	console.log(reduceAncestors(ph, sharedDNA , 0) / 4); // → 0.00049

Finding % of known ancestors, for a given person, who lived past 70:
	
	function countAncestors(person , test) {
		function combine(person, fromMother, fromFather) {
			var thisOneCounts = test(person);
			return fromMother + fromFather + (thisOneCounts ? 1 : 0);
		}
		return reduceAncestors(person , combine , 0);
	}

	function longLivingPercentage(person) {
		var all = countAncestors(person , function(person) {
			return true;
		});
		var longLiving = countAncestors(person , function(person) {
			return (person.died - person.born) >= 70;
		});
		return longLiving / all;
	}
	
	console.log(longLivingPercentage(byName["Emile Haverbeke"])); // → 0.145

Binding:
	add = function(a, b, c) {
		return a + b + c;
	}

	var add1 = add.bind(null, 1);
	add1(2, 3);

	var add11 = add.bind(null, 1, 1);
	add11(2);

	var concat = function() {
		return this.a + this.b + this.c;
	}

	var obj = {a: 'a', b: 'b', c: 'c'};

	var concatObj = concat.bind(obj);
	concatObj();

	//Can't override bound object
	var otherObj = {a: '1', b: '2', c: '3'};
	concatObj.call(otherObj);
	concatObj.apply(otherObj);

Exercises:

- Flattening an array of arrays:
	arrays = [
		[1, 2, 3],
		[4, 5, 6],
		[7, 8, 9]
	];

	// [1, 2, 3, 4, 5, 6, 7, 8, 9]

	console.log(arrays.reduce(function(flat, current) {
  		return flat.concat(current);
	}));

- Mother-child age difference:
	function average(array) {
	  	function plus(a, b) { return a + b; }
	  	return array.reduce(plus) / array.length;
	}

	var byName = {};
	ancestry.forEach(function(person) {
	  	byName[person.name] = person;
	});

	var differences = ancestry.filter(function(person) {
  		return byName[person.mother] != null;
	}).map(function(person) {
  		return person.born - byName[person.mother].born;
	});

	console.log(average(differences)); // → 31.2

- Historical life expectancy
	function groupBy(array, groupOf) {
	  var groups = {};
	  array.forEach(function(element) {
	    var groupName = groupOf(element);
	    if (groupName in groups)
	      groups[groupName].push(element);
	    else
	      groups[groupName] = [element];
	  });
	  return groups;
	}

	var byCentury = groupBy(ancestry, function(person) {
	  return Math.ceil(person.died / 100);
	});

	for (var century in byCentury) {
	  var ages = byCentury[century].map(function(person) {
	    return person.died - person.born;
	  });
	  console.log(century + ": " + average(ages));
	}

- Every and then some






6. Secret Life of Objects:

- A prototype is another object that is used as a fallback source of properties.

- Object.prototype sits at the top of the prototype chain.

- console.log(Object.getPrototypeOf({}) == Object.prototype); 
  // → true
  console.log(Object.getPrototypeOf(Object.prototype));
  // → null

- console.log(Object.getPrototypeOf(isNaN) == Function.prototype);
  // → true
  console.log(Object.getPrototypeOf([]) == Array.prototype);
  // → true

- All properties that we create by simply assigning to them are enumer- able.
- The standard properties in Object.prototype are all nonenumerable, which is why they do not show up in such a for/in loop.

- Define our own nonenumerable properties by using the Object.defineProperty function.
	Object.defineProperty(Object.prototype , "hiddenProperty", {enumerable: false, value: "hi"});


