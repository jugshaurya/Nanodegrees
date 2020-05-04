### Syntax

- Let vs const
- String literals.interploation
  In ES6, you can extract data from arrays and objects into distinct variables using destructuring.
  bject literal shorthand

You’ve probably written code where an object is being initialized using the same property names as the variable names being assigned to them.

But just in case you haven’t, here’s an example.

```js
let type = "quartz";
let color = "rose";
let carat = 21.29;

const gemstone = {
  type: type,
  color: color,
  carat: carat,
};

console.log(gemstone);
```

Prints: Object {type: "quartz", color: "rose", carat: 21.29}

Do you see the repetition? Doesn't type: type, color: color, and carat:carat seem redundant?

The good news is that you can remove those duplicate variables names from object properties _if_ the properties have the same name as the variables being assigned to them.

- for-of-loop
- spread Operator - get values out of array
- rest Operator, fill array with values, earlier argument was a keyword in functions which is an array of all the passed arguments.
  Earlier:

```js
function sum() {
  let total = 0;
  for (const argument of arguments) {
    total += argument;
  }
  return total;
}
```

Now:

````js
function sum(...nums) {
  let total = 0;
  for(const num of nums) {
    total += num;
  }
  return total;
}```
````

## this

![](./images/i.png)
![](./images/j.png)
![](./images/k.png)
![](./images/l.png)
![](./images/m.png)

## Default Params

- with array destructring
- with object destructring

## Classes

- Javascript is not a class-based language
- Javascript implements classes using regular functions only and creates an object using `new` keyword.
- Javascript links objects together with prototypal inheritance.
- ES6 classes hide the fact that prototypal inheritance is actually going on under the hood, let's quickly look at how to create a "class" with ES5 code:

```js
function Plane(numEngines) {
  this.numEngines = numEngines;
  this.enginesActive = false;
}

// methods "inherited" by all instances
Plane.prototype.startEngines = function () {
  console.log("starting engines...");
  this.enginesActive = true;
};

var richardsPlane = new Plane(1);
richardsPlane.startEngines();

var jamesPlane = new Plane(4);
jamesPlane.startEngines();
```

###### Things to note:

- the constructor function is called with the new keyword
- the constructor function, by convention, starts with a capital letter
- the constructor function controls the setting of data on the objects that will be created
- "inherited" methods are placed on the constructor function's prototype object

- ES6 Classes
  Here's what that same Plane class would look like if it were written using the new class syntax:

```js
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  startEngines() {
    console.log("starting engines…");
    this.enginesActive = true;
  }
}
```

### Proof

```js
typeof Plane; // function
```

## Static methods

To add a static method, the keyword static is placed in front of the method name. Look at the badWeather() method in the code below.

```js
class Plane {
  constructor(numEngines) {
    this.numEngines = numEngines;
    this.enginesActive = false;
  }

  static badWeather(planes) {
    for (plane of planes) {
      plane.enginesActive = false;
    }
  }

  startEngines() {
    console.log("starting engines…");
    this.enginesActive = true;
  }
}
// See how badWeather() has the word static in front of it while startEngines() doesn't? That makes badWeather() a method that's accessed directly on the Plane class, so you can call it like this:

Plane.badWeather([plane1, plane2, plane3]);
```

## Benefits of classes

- Less setup
- There's a lot less code that you need to write to create a function
- Clearly defined constructor function
- Inside the class definition, you can clearly specify the constructor function.
- Everything's contained, All code that's needed for the class is contained in the class declaration. Instead of having the constructor function in one place, then adding methods to the prototype one-by-one, you can do everything all at once!

## Subclasses with ES6

Now that we've looked at creating classes in JavaScript. Let's use the new super and extends keywords to extend a class.

```js
class Tree {
  constructor(
    size = "10",
    leaves = { spring: "green", summer: "green", fall: "orange", winter: null }
  ) {
    this.size = size;
    this.leaves = leaves;
    this.leafColor = null;
  }

  changeSeason(season) {
    this.leafColor = this.leaves[season];
    if (season === "spring") {
      this.size += 1;
    }
  }
}

class Maple extends Tree {
  constructor(syrupQty = 15, size, leaves) {
    super(size, leaves);
    this.syrupQty = syrupQty;
  }

  changeSeason(season) {
    super.changeSeason(season);
    if (season === "spring") {
      this.syrupQty += 1;
    }
  }

  gatherSyrup() {
    this.syrupQty -= 3;
  }
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason("fall");
myMaple.gatherSyrup();
myMaple.changeSeason("spring");
```

Both Tree and Maple are JavaScript classes. The Maple class is a "subclass" of Tree and uses the extends keyword to set itself as a "subclass". To get from the "subclass" to the parent class, the super keyword is used. Did you notice that super was used in two different ways? In Maple's constructor method, super is used as a function. In Maple's changeSeason() method, super is used as an object!

**Compared to ES5 subclasses**
Let's see this same functionality, but written in ES5 code:

```js
function Tree(size, leaves) {
  this.size = (typeof size === "undefined")? 10 : size;
  const defaultLeaves = {spring: 'green', summer: 'green', fall: 'orange', winter: null};
  this.leaves = (typeof leaves === "undefined")?  defaultLeaves : leaves;
  this.leafColor;
}

Tree.prototype.changeSeason = function(season) {
  this.leafColor = this.leaves[season];
  if (season === 'spring') {
    this.size += 1;
  }
}

function Maple (syrupQty, size, leaves) {
  Tree.call(this, size, leaves);
  this.syrupQty = (typeof syrupQty === "undefined")? 15 : syrupQty;
}

Maple.prototype = Object.create(Tree.prototype);
Maple.prototype.constructor = Maple;

Maple.prototype.changeSeason = function(season) {
  Tree.prototype.changeSeason.call(this, season);
  if (season === 'spring') {
    this.syrupQty += 1;
  }
}

Maple.prototype.gatherSyrup = function() {
  this.syrupQty -= 3;
}

const myMaple = new Maple(15, 5);
myMaple.changeSeason('fall');
myMaple.gatherSyrup();
myMaple.changeSeason('spring');
Both this code and the class-style code above achieve the same functionality.

```

## Built-ins

- Symbols
- Interable and Interator
- Sets
- Maps
- Promises

## Symbols

![](./images/n.png)
![](./images/o.png)

### Iterables

- In order for an object to be iterable it must contain a `default iterator method`. This method will define how the object should be iterate
- **The iterator method**, which is available via the constant `[Symbol.iterator]`, is a zero arguments function that returns an iterator object.

- An object becomes an iterator when it implements the .next() method. The .next() method is a zero arguments function that returns an object with two properties:

`value` : the data representing the next value in the sequence of values within the object
`done` : a boolean representing if the iterator is done going through the sequence of values

- If done is true, then the iterator has reached the end of its sequence of values.
- If done is false, then the iterator is able to produce another value in its sequence of values.
  Here’s the example from earlier, but instead we are using the array’s default iterator to step through the each value in the array.

```js
const digits = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
// the iterator method, which is available via the constant [Symbol.iterator], is a zero arguments function that returns an iterator object.
const arrayIterator = digits[Symbol.iterator]();


console.log(arrayIterator.next());
console.log(arrayIterator.next());
console.log(arrayIterator.next());

// output
Object {value: 0, done: false}
Object {value: 1, done: false}
Object {value: 2, done: false}
```

## Sets

- In ES6, there’s a new built-in object that behaves like a mathematical set and works similarly to an array. This new object is conveniently called a "Set". The biggest differences between a set and an array are:

- Sets are not indexed-based - you do not refer to items in a set based on their position in the set
  items in a Set can’t be accessed individually
- Basically, a Set is an object that lets you store unique items. You can add items to a Set, remove items from a Set, and loop over a Set. These items can be either primitive values or objects.

**How to Create a Set**
There’s a couple of different ways to create a Set. The first way, is pretty straightforward:

```js
const games = new Set();
console.log(games);
// output
Set {}
```

This creates an empty Set games with no items.

If you want to create a Set from a list of values, you use an array:

```js
const games = new Set(['Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart', 'Super Mario Bros.']);
console.log(games);
// output
Set {'Super Mario Bros.', 'Banjo-Kazooie', 'Mario Kart'}
```

##### Add/Delete

```js
const games = new Set([
  "Super Mario Bros.",
  "Banjo-Kazooie",
  "Mario Kart",
  "Super Mario Bros.",
]);

games.add("Banjo-Tooie");
games.add("Age of Empires");
games.delete("Super Mario Bros.");
```

> > TIP: If you attempt to .add() a duplicate item to a Set, you won’t receive an error, but the item will not be added to the Set. Also, if you try to .delete() an item that is not in a Set, you won’t receive an error, and the Set will remain unchanged.

> > .add() returns the Set if an item is successfully added. On the other hand, .delete() returns a Boolean (true or false) depending on successful deletion.

- Use the .size property to return the number of items in a Set:
- Use the .has() method to check if an item exists in a Set.

**Retrieving All Values**
Finally, use the .values() method to return the values in a Set. The return value of the .values() method is a SetIterator object.

```js
console.log(months.values());
SetIterator {'January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'}
```

> > TIP: The .keys() method will behave the exact same way as the .values() method by returning the values of a Set within a new Iterator Object. The .keys() method is an alias for the .values() method for similarity with maps. You’ll see the .keys() method later in this lesson during the Maps section.

##### Using a for...of Loop

An easier method to loop through the items in a Set is the for...of loop.

```js
const colors = new Set([
  "red",
  "orange",
  "yellow",
  "green",
  "blue",
  "violet",
  "brown",
  "black",
]);
for (const color of colors) {
  console.log(color);
}
```

## Maps

![](./images/p.png)

```js
const employees = new Map();
console.log(employees);
```

###### Modifying Maps

Unlike Sets, you can’t create Maps from a list of values; instead, you add key-values by using the Map’s .set() method.

```js
const employees = new Map();

employees.set('james.parkes@udacity.com', {
    firstName: 'James',
    lastName: 'Parkes',
    role: 'Content Developer'
});
employees.set('julia@udacity.com', {
    firstName: 'Julia',
    lastName: 'Van Cleve',
    role: 'Content Developer'
});
employees.set('richard@udacity.com', {
    firstName: 'Richard',
    lastName: 'Kalehoff',
    role: 'Content Developer'
});

console.log(employees);
Map {'james.parkes@udacity.com' => Object {...}, 'julia@udacity.com' => Object {...}, 'richard@udacity.com' => Object {...}}
```

The .set() method takes two arguments. The first argument is the key, which is used to reference the second argument, the value.

To remove key-value pairs, simply use the .delete() method.

```js
employees.delete('julia@udacity.com');
employees.delete('richard@udacity.com');
console.log(employees);
Map {'james.parkes@udacity.com' => Object {firstName: 'James', lastName: 'Parkes', role: 'Course Developer'}}
```

Again, similar to Sets, you can use the .clear() method to remove all key-value pairs from the Map.

```js
employees.clear()
console.log(employees);
Map {}
```

> > TIP: If you .set() a key-value pair to a Map that already uses the same key, you won’t receive an error, but the key-value pair will overwrite what currently exists in the Map. Also, if you try to .delete() a key-value that is not in a Map, you won’t receive an error, and the Map will remain unchanged.

> > The .delete() method returns true if a key-value pair is successfully deleted from the Map object, and false if unsuccessful. The return value of .set() is the Map object itself if successful.
