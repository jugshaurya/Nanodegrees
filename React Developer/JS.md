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

## Functions
