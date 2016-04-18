#  this keyword binding with context object:
1. implicit (owning or containing object)
2. default (global object in loose mode)
3. explicit (call/apply/bind)
4. new (newly created object)

# Order of precedence of what the context object will be:
1. new
2. explicit
3. implicit
4. default

# Closure:
When a function remembers its lexical scope even when it is executed outside of its lexical scope.

# Classic Module Pattern:
1. Outer wrapper/enclosing function/IIFE call.
2. One or more inner function returned from wrapper function/revealing public api.

# new operator:
1. brand new object gets created
2. object gets linked to its constructor prototype using internal linkage property called [[Prototype]] as per spec. The public version of this property is __proto__ which is called dunder property.
3. context gets set to this which is newly created object
4. return this if nothing is returned explicitly

Object.create() does 1 and 2 only.

# Prototype:
Every single object is built by a construction function
A constructor makes an object linked to its own prototype

function Foo(who) {
	this.me = who;
}

Foo.prototype.identity = function () {
	return 'I am ' + this.me;
}

var a1 = new Foo('a1');
var a2 = new Foo('a2');

a2.speak = function () {
	alert('Hello' + this.identity() + '.');
}

a1.constructor == Foo;
a1.constructor == a2.constructor;
a1.__proto__ == Foo.prototype;
a1.__proto__ == a2.__proto__;

a1.__proto__ == Object.getPrototypeOf(a1); //ES5, IE9
a2.constructor == Foo;
a1.__proto__ == a2.__proto__;
a2.__proto__ == a2.constructor.prototype; //IE8 and below, ES3
Both constructor and prototype properties are writable properties


