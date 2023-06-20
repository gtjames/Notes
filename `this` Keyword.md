## `this` Object Methods
[link to article](https://javascript.info/object-methods)

Shorter syntax to create a method in an object literal. There are subtle differences related to inheritance, but shorter syntax is usually preferred.

```javascript

user = {
	sayHi: function() {
		alert('Hello!')
	}
};

// same as 

user = {
	sayHi() {
		alert('Hello!');
	}
}
```

### `this` in methods
It’s common that an object method needs to access the information stored in the object to do its job.

For instance, the code inside `user.sayHi()` may need the name of the `user`.

**To access the object, a method can use the `this` keyword.**

```javascript
let user = {
	name: 'John'
	age: 30,
	
	sayHi() {
	// "this" is the current object
		alert(this.name);
	}
}

user.sayHi() //John
```

Here during the execution of `user.sayHi()`, the value of `this` will be `user`.

### `this` is not bound
The rule is simple: if `obj.f()` is called, then `this` is `obj` during the call of `f`. So it’s either `user` or `admin` in the example above.

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };
 

function sayHi() {
	alert( this.name );
}

// use the same function in two objects
user.f = sayHi;
admin.f = sayHi;

  

// these calls have different this
// "this" inside the function is the object "before the dot"
user.f(); // John (this == user)
admin.f(); // Admin (this == admin)
  

admin['f'](); // Admin (dot or square brackets access the method – doesn't matter)
```

In JavaScript `this` is “free”, its value is evaluated at call-time and does not depend on where the method was declared, but rather on what object is “before the dot”. Other programming languages have `this` always bound. 

### Arrow Functions have no`this`

Arrow functions are special: they don’t have their “own” `this`. If we reference `this` from such a function, it’s taken from the outer “normal” function.

```javascript
let user = {
	firstName: "Ilya",
	sayHi() {
		let arrow = () => alert(this.firstName);
		arrow();
	}
};

  

user.sayHi(); // Ilya
```

## Summary

-   Functions that are stored in object properties are called “methods”.
-   Methods allow objects to “act” like `object.doSomething()`.
-   Methods can reference the object as `this`.

The value of `this` is defined at run-time.

-   When a function is declared, it may use `this`, but that `this` has no value until the function is called.
-   A function can be copied between objects.
-   When a function is called in the “method” syntax: `object.method()`, the value of `this` during the call is `object`.

Please note that arrow functions are special: they have no `this`. When `this` is accessed inside an arrow function, it is taken from outside.

If you are still confused, check out [this article](https://zellwk.com/blog/this/)

