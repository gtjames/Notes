>Object-oriented programming (OOP for short) is a style of programming that involves separating the code into objects that have properties and methods.



# Object-Oriented Programming
1. Encapsulation
2. Polymorphism
3. Inheritance
4. Classes



## Encapsulation
*Concept of Encapsulation*: the inner workings are kept hidden inside the object and only the essential functionalities are exposed to the end user.
In OOP, this involves **keeping all the programming logic inside an object and making methods available to implement the functionality**, without the outside world needing to know _how_ it’s done.

## Polymorphism
_Concept of polymorphism_: the same process can be used for different objects.
In OOP, this means various objects can share the same method, but also have the ability to __override shared methods__ with a more specific implementation.

## Inheritance
_Concept of inheritance_: taking the features of one object then adding some new features.
In OOP, this means we can take an object that already exists and __inherit all its properties and methods__. We can then improve on its functionality by adding new properties and methods.

## Classes
Many object-oriented languages, such as Java and Ruby, are known as **class-based** languages. This is because they use a class to define a blueprint for an object. Objects are then created as an instance of that class, and inherit all the properties and methods of the class.

JavaScript didn't have classes before ES6, and used the concept of using actual objects as the blueprint for creating more objects. This is known as a **prototype-based** language. Even though JavaScript has classes now, it still uses the prototypal model in the background.

## Constructor Functions

JavaScript allows you to create an object with the `object literal notation`
```javascript
const dice = {
	sides: 6,
	roll(), {
		return Math.floor(this.sides * Math.random() + 1)
	}
}
```

Alternatively, create an object with the _constructor function_

```javascript
const Dice = function(sides = 6) {
	this.sides = sides
	this.roll = function() {
		return Math.floor(this.sides * Math.random() + 1)
	}
}
```

The keyword `this` represents the object that will be returned by the constructor.

Create an instance of `Dice` with the `new` operator
```javascript
const redDice = new Dice();
<< Dice { sides: 6, roll: [Function] }
```

> Parenthesis are not required when instantiating an object unless we want to pass different arguments from the default ones.

If we want to check that `redDice` is an instance of `Dice` we can type
```javascript
redDire instanceof Dice
<< true
```

## Built-in Constructor Functions
JavaScript contains a number of built-in constructor functions such as `Object`, `Array`, and `Function` that can be used to create objects, arrays and functions instead of literals.

Create an object literal
```javascript
const literalObject = {}
<< {}
```

Crete an object with the `Object` constructor function
```javascript
constructedObject = new Object()
<< {}
```

Create an array with literal syntax
```javascript
const literalArray = [1, 2, 3]
<< [1, 2, 3]
```

Create an array with `Array` constructor function
```javascript
constructedArray = new Array(1, 2, 3)
<< [1, 2, 3]
```

## ES6 Class Declarations

ES6 introduced _class declaration syntax_.
Put the variables inside the constructor and the methods outside.

```javascript
class Dice {
	constructor(sides=6) {
		this.sides = sides;	
	}
	roll() {
		return Math.floor(this.sides * Math.random() + 1)
	}
}
```

> The name of a class is capitilized by convention

Create an instance of the class with the `new`
```javascript
const blueDice = new Dice(20)
<< Dice { sides: 20 }
```

The class declaration syntax is syntactic sugar as it is implemented as a constructor function in the background.

Best practice: use class declaration syntax
+ More succint
+ Easier to read
+ All code implicitly in `strict` mode by default
+ Avoid a number of pitfalls associated with constructor functions

Ex:
```javascript
// Using constructor function - noDice is just set to undefined without any warning
const noDice = Dice();
noDice
<< undefined

// Using class - an error is thrown
const noDice = Dice();
<< TypeError: Class constructor Dice cannot be invoked without 'new'
```

Read more about classes in JavaScript [here](https://www.sitepoint.com/object-oriented-javascript-deep-dive-es6-classes/)

## The Constructor Property
All objects have a `constructor` property that returns the constructor function that created it. The same behavior happens when you create an object wit the object literal syntax.

```javascript
blueDice.constructor<< [Function: Dice]

const literalObject = {};
<< {}
literalObject.constructor
<< [Function: Object]
```

If you don't know the name of the original class, you can create a copy of an object from another one referring to its constructor function
```javascript
const greenDice = new redDice.constructor(10);

greenDice instanceOf Dice
<< true
```

## Static Methods
The `static` keyword can be used in class declarations to create a static method. These are sometimes called class methods in other programming languages. They can

__A static method is called by the class directly rather than by instances of the class.__

```javascript
class Dice {
    constructor(sides=6) {
        this.sides = sides;
    }

    roll() {
        return Math.floor(this.sides * Math.random() + 1)
    }

    static description() {
        return 'A way of choosing random numbers'
    }
}
```

This static mathod can be called by the class like so:
```javascript
Dice.description()
<< 'A way of choosing random numbers'
```

> Static methods are not available to instances of the class.

## Prototypal Inheritance

Every class has a prototype property that is shared by every instance of the class. So any properties or methods of a class’s prototype can be accessed by every object instantiated by that class.

### Prototypal Property

What if you want to augment the class with extra methods and properties after it has been created? It turns out that you can still do this using the **prototype** property of the class.

All classes and constructor functions have a `prototype` property that returns an object:
```javascript
Turtle.prototype;
<< Turtle {}
```

All instances of the the `Turtle` class share all the properties and methods of its prototype. This means they can call any methods of the prototype and access any of its properties. Since the prototype is just an object, we can add new properties by assignment:

```javascript
Turtle.prototype.weapon = 'Hands';<< 'Hands'
```

```javascript
Turtle.prototype.attack = function(){return `Feel the power of my ${this.weapon}!`;}<< [Function]
```

Now if we create a new `Turtle` instance, we can see that it inherits the `weapon` property and `attack()` method from the `Turtle.prototype` object, as well as receiving the `name` property and `sayHi()` method from the class declaration:

```javascript
const raph = new Turtle('Raphael');
raph.name<< 'Raphael'
raph.sayHi()<< 'Hi dude, my name is Raphael'
raph.weapon<< 'Hands'
raph.attack()<< 'Feel the power of my Hands!'
```

Bottom line is you can add methods and variables calling the `prototype` property to a class.

### Finding out the Prototype

Constructor's function `prototype` property
```javascript
raph.constructor.prototype;
<< Turtle { attack: [Function], weapon: 'Hands' }
```

Use the `Object.getPrototypeOf()` method
```javascript
Object.getPrototypeOf(raph);
<< Turtle { attack: [Function], weapon: 'Hands' }
```

Every object also has a `isPrototypeOf()` method that returns a boolean to check if it’s the prototype of an instance:
```javascript
Turtle.prototype.isPrototypeOf(raph)<< true
```

### Own Properties and Prototype Properties
> In the previous example, the object `raph` had a `name` property that it inherited from the class declaration, and a `weapon` property that it inherited from the prototype property.

Every object has a `hasOwnProperty()` method that can be used to check if a method is its own property, or is inherited from the prototype:

```javascript
raph.hasOwnProperty('name');
<< true

raph.hasOwnProperty('weapon');
<< false
```

So what’s the difference between an object's own properties and prototype properties? Prototype properties are shared by _every_ instance of the `Turtle` class. 

This means that it only__ exists in memory in one place, which is more efficient than each instance having its own value__. This is particularly useful for any properties that are the same.

### Prototype is Live!
The `prototype` object is live, so if a new property or method is added to the prototype, any instances of its class will inherit the new properties and methods automatically, even if that instance has already been created.

It is not possible to overwrite the prototype by assigning it to a new object literal if class declarations are used:

```javascript
Turtle.prototype = {}<< {}
```

Even though it looks like the prototype has been reassigned to an empty object literal, we can see see it hasn't actually changed:

```javascript
Turtle.prototype<< Turtle { attack: [Function], weapon: 'Feet' }
```

It _is_ possible to do this if constructor functions are used, and it can cause a lot of headaches if you accidentally redefine the prototype.

### Overwriting Protype Properties
An object instance can overwrite any properties or methods inherited from its prototype by simply assigning a new value to them. For example, we can give our turtles their own weapon properties:

```javascript
leo.weapon = 'Katana Blades';<< 'Katana Blades';
raph.weapon = 'Sai';<< 'Sai'
don.weapon = 'Bo Staff';<< 'Bo Staff'
```

These properties will now become an 'own property' of the instance object:

```javascript
leo<< Turtle { name: 'Leonardo', weapon: 'Katana Blades' }
```

Any own properties will take precedence over the same `prototype` property

### When Should I use prototypes?
+ New properties and methods after the class has been declared
+ Define properties that will stay the same for each instance
+ Methods are good candidates for the prototype
+ ***NEVER*** use arrays or objects as a default value in prototype

## Public and Private Methods

Default: Public methods in JS. Public means that they can be queried and reassigned. 

It is not ideal to have methods and variables public. We can use _variable scope_ to keep properties and methods safe. We will provide _getter_ and _setter_ method to safely access them.

Let's create a new class `Turtle` where the `_color` variable is private
```javascript
class Turtle {
    constructor(name,color) {
        this.name = name;
        let _color = color;
        this.setColor = color => { return _color = color; }
        this.getColor = () => _color;
    }
}
```

The `_color` property is created as a variable inside the scope of the constructor function inside the class declaration. 
This makes it impossible to access outside of this scope. 

The getColor() and setColor() methods are known as _getter and setter methods_ and ___they form a closure over this variable___ and provide controlled access to the property instead:

```javascript
raph = new Turtle('Raphael','Red');
<< Turtle { name: 'Raphael', setColor: [Function], getColor: [Function] }

raph.getColor(); // my example works without parenthesis. Strange
<< 'Red'

raph.setColor(4);
<< 4
```

You can now check the user input and enforce specific rule
```javascript
this.setColor = (color) => {
if(typeof color === 'string'){
    return _color = color;
    } else {
        throw new Error('Color must be a string');
    }
}

raph.setColor(4);
<< Error: Color must be a string
```

## Inheritance

### The Prototype Chain
You can see the prototype chain in this way
```javascript
Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(raph)))
<< null
```

At one point you will get null when you arrive at the top of the chain, in this case at the `Object()` constructor function.

## The Object Constructor
> All objects ultimately inherit from the prototype of the `Object()` constructor function.

When you try to access a method, JavaScript will run the prototype chain to see if that method exits. If it arrives at `Object()` and the method is not there, it will throw an error.
```javascript
raph.makePizza();
<< TypeError: raph.makePizza is not a function
```

The prototype of the `Object` constructor function has a large number of methods that are inherited by all objects. The reason why the prototype appears as an empty object literal is because __all of its methods are not enumerable__.

### Enumerable Properties
> Properties of objects in JavaScript are said to be _enumerable_ or _non-enumerable_. If they aren't enumerable, this means they will not show up when a `for-in` loop is used to loop through an object’s properties and methods.

There is a method called `propertyIsEnumerable()` that every object has (because it’s a method of `Object.prototype`) that can be used to check if a property is enumerable.

```javascript
Turtle.prototype.propertyIsEnumerable('eat');
<< true

Object.prototype.propertyIsEnumerable('toString');
<< false

Object.prototype.propertyIsEnumerable('propertyIsEnumerable');
<< false
```

### Inheritance Using `Extends`
A class can inherit from another class using the `extends` keyword in a class declaration.

Original class
```javascript
class Turtle {
    constructor(name) {
        this.name = name;
        }
    sayHi() {
        return `Hi dude, my name is ${this.name}`;
    }

    swim() {
        return `${this.name} paddles in the water`;
    }
}
```

New class extending `Turtle`
```javascript
class NinjaTurtle extends Turtle {
	constructor(name) {
		super(name);
		this.weapon = 'hands';
	}
	attack() { return `Feel the power of my ${this.weapon}!` }
}
```

> Inside the child class declaration, the keyword `super` refers to the parent class, and can be used to access any properties and call any methods of the parent class. In the example above we use it to call the constructor function of the `Turtle` class.

## Polymorphism

> The concept of polymorphism means that different objects can have the same method, but implement it in different ways. Polymorphism means that objects are able to override this method with a more specific implementation.

The number, string, and boolean primitive types that we met way back in Chapter 2 have their own corresponding constructor functions: `Number`, `String`, and `Boolean` respectively. 

They do not produce a primitive type. Primitives do not have methods. The wrapper objects above are provided in the background to provide primitives with methods.

It’s often a useful exercise to override the `toString()` method using the prototype, so something more meaningful is displayed. For example, we could edit the `Turtle()` class declaration so it includes a more descriptive `toString()` method:

```javascript
class Turtle {
    // other turtle methods here

    toString() {
        return `A turtle called ${this.name}`;
    }
}

raph.toString();
<< 'A turtle called Raphael'
```

## Adding Methods to Built-in Objects

You can add more methods tot he built-in JS objects - for ex `Number`, `String`, `Array`.
It is frowned upon on the Js comunity but pretty used in the Ruby one. This practice it called _[monkey patching](https://www.sitepoint.com/pragmatic-monkey-patching/)_.

As an example, we can add `isOdd()` and `isEven()` methods to the `Number` wrapper object’s prototype. These methods will then be available to number primitives:

```javascript
Number.prototype.isEven = function() {
    return this%2 === 0;
}

Number.prototype.isOdd = function() {
    return this%2 === 1;
}

42..isEven();
<< true

765234..isOdd();
<< false
```

Useful methods on `Array`
```javascript
Array.prototype.first = function() {
    return this[0];
}

Array.prototype.last = function() {
    return this[this.length -1];
}

const turtles = ['Leonardo', 'Donatello', Michaelangelo', 'Raphael'];

turtles.first();
<< 'Leonardo'

turtles.last();
<< 'Raphael'
```

Create a `delete()` built-in method on `Array`
```javascript
Array.prototype.delete = function(i) {
    return self.splice(i,1);
}
```

Create a support for IE8 and older for the method `trim()`
```javascript
String.prototype.trim = String.prototype.trim || function() { 
    return this.replace(/^\s+|\s+$/,''); 
}

' hello '.trim();
<< 'hello'
```

### Property Attributes and Descriptors

All object properties have the following attributes stored in a property descriptor:

+ `value` ― This is the value of the property and is `undefined` by default
+ `writable` ― This boolean value shows whether a property can be changed or not, and is false by default
+ `enumerable` ― this boolean value shows whether a property will show up when the object is displayed in a `for in` loop, and is `false` by default
+ `configurable` ― this boolean value shows whether you can delete a property or change any of its attributes, and is `false` by default.

For example, consider the following object, which has the single property of `name`:
```javascript
const me = { name: 'DAZ' };

// Property descriptors look like this:
{ value: 'DAZ', writable: true, enumerable: true, configurable: true }
```

Add property by assignment
```javascript
me.age = 21;
<< 21
```

The disadvantage with this is that it can only be used to set the `value` attribute of the property. You cannot set the other properties.

### Getting and Setting Property Descriptors

The `Object()` constructor function has a number of methods for getting and defining property descriptors. We can see these values using the `Object.getOwnPropertyDescriptor()` method:
```javascript
Object.getOwnPropertyDescriptor(me,'name');

<< { value: 'DAZ',
writable: true,
enumerable: true,
configurable: true }
```

Instead of using assignment, we can add properties to an object using the `Object.defineProperty()` method.

```javascript
Object.defineProperty(me, 'eyeColor', { value: 'blue', writable: false, enumerable: true }); 

<< { name: 'DAZ', age: 21, eyeColor: 'blue' }
```

### Getters and Setters
An object property descriptor can have `get()` and `set()` *methods* instead of a *value attribute*. They cannot have both.

The `get()` and `set()` methods can be used to control how a property is set using assignment and the value that is returned when a property is queried.

```javascript
me.age = 21;
me.retirementAge = 65;

Object.defineProperty(me, 'yearsToRetirement',{
    get() {
        if(this.age > this.retirementAge) { return 0; }
        else { return this.retirementAge - this.age; }
    },
    set(value) {
        this.age = this.retirementAge - value;
        return value;
    }
});
```

The `get` and `set` property descriptors are particularly useful for controlling the getting and setting of properties in classes.

Ex. Dice that has more granular control over the sides assignment
```javascript
class Dice {
constructor(sides=6){    
    Object.defineProperty(this, 'sides', {
        get() {
        return `This dice has ${sides} sides`;
        },
        set(value) {
        if(value > 0) {
            sides = value;
            return sides;
        } else {
            throw new Error('The number of sides must be positive');
        }
        }
    });

    this.roll = function() {
        return Math.floor(sides * Math.random() + 1)
    }
    }
}
```

## Creating Objects from Other Objects

> It’s possible to avoid using classes altogether, and create new objects based on another object that acts as a 'blueprint' or prototype instead.

The `Object()` constructor function has a method called `create` that can be used to create a new object that is an exact copy of the object that is provided as an argument. The object that is provided as the argument acts as the prototype for the new object.

We can create an instance of `Human` using the `Object.create()` method:

```javascript
const Human = {
arms: 2,
legs: 2,
walk() { console.log('Walking'); }
}

const lois = Object.create(Human);

lois.arms
<< 2

lois.legs
<< 2

lois.walk()
<< Walking
```

An alternative way is to add a second argument to the `Object.create()` method containing properties that are to be added to the new object. Properties must be added as property descriptors. 

```javascript
const jimmy = Object.create(Human, { name: { value: 'Jimmy Olsen', enumerable: true }, job: { value: 'Photographer', enumerable: true } });
```

### Object-Based Inheritance
The `Human` object can also act like a ‘super-class’, and become the prototype of another object called `Superhuman`.

Create an object and assign a new method and properties with the assigning syntax:
```javascript
const Superhuman = Object.create(Human);

Superhuman.change = function() {
return `${this.realName} goes into a phone box and comes out as ${this.name}!`;
};

Superhuman.name = 'Name Needed';
<< 'Name Needed'

Superhuman.realName = 'Real Name Needed';
<< 'Real Name Needed'
```

This way is a little long winded. Let's create a `init` method to initialize the object
```javascript
Superhuman.init = function(name,realName){
    this.name = name;
    this.realName = realName;
    this.init = undefined; // this line removes the init function, so it can only be called once
    return this;
}

const batman = Object.create(Superhuman);
batman.init('Batman','Bruce Wayne');

batman.change();
<< 'Bruce Wayne goes into a phone box and comes out as Batman!'
```

It can also be initialized in just one line
```javascript
const aquaman = Object.create(Superhuman).init('Aquaman', 'Arthur Curry');

aquaman.change();
<< 'Arthur Curry goes into a phone box and comes out as Aquaman!'
```

## Mixins
> A mixin is a way of adding properties and methods of some objects to another object without using inheritance.

Basic mixin functionality is provided by the `Object.assign()` method.
```javascript
const a = {};

const b = { name: 'JavaScript' };

Object.assign(a,b);
<< { name: 'JavaScript' }

a.name
<< 'JavaScript'
```

There is a problem with this method, however. If any of the properties being mixed in are arrays or nested objects, only a shallow copy is made (copied by reference).

> When objects are copied by assignment, they are only copied by reference. This means that another object is not actually created in memory; the new reference will just point to the old object. Any changes that are made to either objects will affect both of them. [..] This is known as making a shallow copy of an object. A deep or hard copy will create a completely new object that has all the same properties as the old object. The difference is that when a hard copy is changed, the original remains the same. But when a shallow copy is changed, the original changes too.

```javascript
const a = {};
const b = { numbers: [1,2,3] };

Object.assign(a,b);
<< { numbers: [1,2,3] }

b.numbers.push(4);
<< 4

b.numbers
<< [1,2,3,4]

a.numbers // This has also changed
<< [1,2,3,4]
```

To avoid only a shallow copy, we're going to create our own `mixin()` function that will assign all properties of an object to another object as a _deep_ copy.

```javascript
function mixin(target,...objects) {
    for (const object of objects) {   
    if(typeof object === 'object') {
        for (const key of Object.keys(object)) {
            if (typeof object[key] === 'object') {
            target[key] = Array.isArray(object[key]) ? [] : {};
            mixin(target[key],object[key]);
            } else {
            Object.assign(target,object);  
            }
        }
        }
    }
    return target;
}
```

Code explanation:
This code looks very complicated at first glance, so let's dive into it and see what’s happening.

The first parameter is the object that we are applying the mixin to. The second parameter uses the rest parameter `...objects` to allow multiple objects to be 'mixed in' at once. These will be available in the function as an array called `objects`.

We then use a `for-of` loop to iterate through each object in this array.

Next we iterate through each property in the object using the `Object.keys()` iterable.

The next line is the important part that ensures a deep copy. The problematic properties that are not deep copied are arrays and objects. Both of these return 'object' when the `typeof` operator is used. If that is the case, we need to do something different than just use `Object.assign()` to copy the property.

If the property is an object, we use a ternary operator to check whether it is an array or an object using the `Array.isArray()` method. If it is an array, then its constructor function will be `Array`. We create a new array literal, otherwise we create a new object literal.

Then we apply the `mixin` method recursively to add each property one at a time to the literal that was just created, instead of just using assignment.

And finally, the `else` statement states that `Object.assign` should still be used for any properties that are not arrays or objects because a shallow copy will work fine for those.

### Using Mixins to Add Properties
We can just mix in an object literal and add both properties at once:

```javascript
mixin(wonderWoman,{ name: 'Wonder Woman', realName: 'Diana Prince' });

wonderWoman.change()
<< 'Diana Prince goes into a phone box and comes out as Wonder Woman'
```

### Using Mixins to Create a `copy()` Function
Create a deep, exact copy of an object
```javascript
function copy(target) {
    const object =  Object.create(Object.getPrototypeOf(target));
    mixin(object,target);
    return object;
}
```

### [Factory Functions](https://www.sitepoint.com/factory-functions-javascript/)
Our `copy()` function can now be used to create a _factory function_ for superheroes. A factory function is a function that can be used to return an object.

```javascript
function createSuperhuman(...mixins) {
    const object = copy(Superhuman);
    return mixin(object,...mixins);
}

const hulk = createSuperhuman({name: 'Hulk', realName: 'Bruce Banner'});

hulk.change()
<< 'Bruce Banner goes into a phone box and comes out as Hulk!'
```

### Using the Mixin Function to Add Modula Functionality
We do not always wait to create a chain of inheritance. The `mixin()` function lets us encapsulate properties and methods in an object, then add them to other objects without the overhead of an inheritance chain being created.

We can use both prototypal inheritance and mixin objects thinking whether an _object is_ something or _it has_ something. 
+ Prototypal: It is something
+ Mixin object: it has something

## Chaining Functions

If a method returns `this`, its methods can be chained together to form a sequence of method calls that are called one after the other. For example, the `superman` object can call all three of the superpower methods at once:

```javascript
const flight = {
    fly() {
        console.log(`Up, up and away! ${this.name} soars through the air!`);
        return this;
    }
}

const superSpeed = {
    move() {
        console.log(`${this.name} can move faster than a speeding bullet!`);
        return this;
    }  
}

const xRayVision = {
    xray() {
        console.log(`${this.name} can see right through you!`);
        return this;
    }  
}

superman.fly().move().xray();

<<  Up, up and away! Superman soars through the air!
    Superman can move faster than a speeding bullet!
    Superman can see right through you!
```

Used in libraries
+ jQuery
+ Jest

Biggest drawback is that it makes code hard to debug.

### Binding `this`

There is a problem with `this` - it loses its scope - when a function is nested inside a function method (ex. callbacks). `this` points to the global object (window) instead of our obejct.

```javascript
superman.friends = [batman,wonderWoman,aquaman]

superman.findFriends = function(){
    this.friends.forEach(function(friend) {
        console.log(`${friend.name} is friends with ${this.name}`);
    }
    );
}

superman.findFriends()

<<  Batman is friends with undefined
    Wonder Woman is friends with undefined
    Aquaman is friends with undefined
```

The `findFriends()` method fails to produce the expected output because `this.name` is actually referencing the `name` property of the global `window` object, which has the value of `undefined`.

### How to solve the problem: `that=this` and `bind(this)`

#### Use `that=this`
A common solution is to set the variable `that` to equal `this`_before_ the nested function, and refer to `that` in the nested function instead of `this`. You might also see `self` or `_this` used to maintain scope in the same way.

Here is the example again, using `that`
```javascript
superman.findFriends = function(){
    const that = this;
    this.friends.forEach(function(friend) {
        console.log(`${friend.name} is friends with ${that.name}`);
    }
    );
}

superman.findFriends();

<<  Batman is friends with Superman
    Wonder Woman is friends with Superman
    Aquaman is friends with Superman
```

#### Use `bind(this)`
The `bind()` method is a method for all functions and is used to set the value of `this` in the function. If `this` is provided as an argument to `bind()` while it’s still in scope, any reference to `this` inside the nested function will be bound to the object calling the original method:

```javascript
superman.friends = [batman,wonderWoman,aquaman]

superman.findFriends = function() {
	this.friends.forEach(function(friend) {
		console.log(`${friend.name} is friends with ${this.name}`);
	}.bind(this);)
}

// It works as expected
```

### Use `for-of` instead of `forEach()`
ES6 introduced the `for-of` syntax for arrays and this does not require a nested function to be used, so `this` remains bound to the `superman` object:
```javascript
superman.findFriends = function() {
    for(const friend of this.friends) {
        console.log(`${friend.name} is friends with ${this.name}`);
    };
}

superman.findFriends();
<<  Batman is friends with Superman
    Wonder Woman is friends with Superman
    Aquaman is friends with Superman
```

### Use Arrow Functions
> Arrow functions were introduced in ES6, and one of the advantages of using them is that **they don't have their own `this` context**, *so `this` remains bound to the original object* making the function call:

```javascript
superman.findFriends = function() {
    this.friends.forEach((friend) => {
        console.log(`${friend.name} is friends with ${this.name}`);
    }
    );
}
// It works as expected
```

Use arrow functions when required in callbacks so you don't have binding problems with this.

## Borrowing Methods from Prototypes

You can borrow a method from another object by creating a reference to it:
```javascript
const fly = superman.fly; // no parenthesis as it is not a func call
```

The method can now be called on another object using the `call` method:
```javascript
fly.call(batman)

<< Up, up and away! Batman soars through the air!
```

### Borrowing Array Methods

Most of these techniques are not needed from ES6 onwards as the `Array.from()` method can be used to turn an array-like object into an array:

```javascript
const argumentsArray = Array.from(arguments);
```

Alternatively, the spread operator can be used to easily turn an array-like object into an array like so:

```javascript
const argumentsArray = [...arguments];
```

When you use *anonymus functions* declared `function () {}` they have the `arguments` array-like object. However, *arrow functions* do not. Here is the workaround to implement the array-like `arguments` with the spread operator:

```javascript
const funcTest = (...arguments) => {
  const argArray = [...arguments]
  console.log(argArray)
}

funcTest('fede', 2, 'hello', [1, 3, 4])

<< ['fede', 2, 'hello', Array(3)]
```

## Composition Over Inheritance
Problems with inheritance:
1. Do not support multiple inheritance
2. 'Gorilla Banana problem' - "You wanted a banana but what you got was a gorilla holding the banana and the entire jungle" (Joe Armstrong, inventor of Erlang). To get the `banana()` method, you inherit the `Gorilla` class and a bunch of things that you don't need along with it. It bloats the program and can be very inefficient.

Composition over inheritance is a design pattern that tries to solve this issue. 

> This approach advocates creating small objects that describe single tasks or behaviors and using them as the building blocks for more complex objects. [...] Composition over inheritance sees objects as building blocks that go together to make other objects rather than classes that are monolithic structures layered on top of each other.

If you use classes: 
+ Make the *skinny* - don't have too many properties or methods.
+ Keep the inheritance chain short
+ Only inherit once
+ Borrow the method instead of inheriting the whole thing

Ex: 
```javascript
banana = Gorilla.prototype.banana;

banana.call(Monkey)
```

If you want to dive deeper in to the subject, [read this article](https://medium.com/javascript-scene/the-two-pillars-of-javascript-ee6f3281e7f3)

## Chapter Summary

-   Object-oriented programming (OOP) is a way of programming that uses objects that encapsulate their own properties and methods.
-   The main concepts of OOP are encapsulation, polymorphism and inheritance.
-   Constructor functions can be used to create instances of objects.
-   ES6 introduced class declarations that use the `class` keyword. These can be used in place of constructor functions.
-   Inside a constructor function or class declaration, the keyword `this` refers to the object returned by the function.
-   All instances of a class or constructor function inherit all the properties and methods of its prototype.
-   The prototype is live, so new properties and methods can be added to existing instances.
-   The prototype chain is used to find an available method. If an object lacks a method, JavaScript will check whether its prototype has the method. If not, it will check that function’s prototype until it finds the method or reaches the `Object` constructor function.
-   Private properties and methods can be created by defining variables using `const` and defining a function inside a constructor function. These can be made public using getter and setter functions.
-   Monkey-patching is the process of adding methods to built-in objects by augmenting their prototypes. This should be done with caution as it can cause unexpected behavior in the way built-in objects work.
-   A mixin method can be used to add properties and methods from other objects without creating an inheritance chain.
-   Methods can be chained together and called in sequence if they return a reference to `this.`
-   Polymorphism allows objects to override shared methods with a more specific implementation.
-   The value of `this` is not retained inside nested functions, which can cause errors. This can be worked around by using `that = this`, using the `bind(this)` method and using arrow functions.
-   Methods can be borrowed from other objects.
-   Composition over inheritance is a design pattern where objects are composed from 'building-block' objects, rather than inheriting all their properties and methods from a parent class.


