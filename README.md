# Different types of loops
Lots of different types of loops, for different purposes, more important to be aware of the different types and what they're good for than the exact syntax. Don't forget array methods like map, filter etc that are effectively very specific loops and can be very useful.

[MDN Loops Deep Dive](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)
[Another GitHub with Demos](https://github.com/Asabeneh/JavaScript-Loops)
[Using the JavaScript while… and do…while loops](http://thenewcode.com/1122/Using-the-JavaScript-while-and-dowhile-loops)
[When to Use hasOwnProperty](https://stackoverflow.com/questions/32422867/when-do-i-need-to-use-hasownproperty)

# for 
- Can do practically everything other more specialized loops can, but best used when you know how many iterations you need
- Common type of loop similar to that in C and Java. 
- Allows you to provide your own code for initializing, checking and changing the value 



Simple:
```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}
```
Decrementing instead of incrementing - you can literally do anything in each of the statements:
```javascript
for (let i = 3; i > 0; i--) {
    console.log(i);
}
```
Infinite loop (all three statements are empty): 
```javascript
// Do not do
for (;;) {
    console.log("Iterate");
}
```
Iterating through two arrays at once - somewhat common production use case
```javascript
let a = ['a', 'b', 'c']
let b = ['x', 'y', 'z']
for (let i = 0; i < a.length; i++) { // You could write 3 instead of a.length with same result
    console.log(`Array a value at index ${i}: ${a[i]} Array b value at index ${i}: ${b[i]}`);
}
```
i not accessible outside loop, if you declare using let:
```javascript
for (let i = 0; i < 3; i++) {
    console.log(i);
}
console.log(i); // Throws an exception: Uncaught ReferenceError: i is not defined
```
i accessible outside loop, if you declare using var (i.e. this is a standard variable declaration):
```javascript
// Don't do this BTW
for (var i = 0; i < 3; i++) {
    console.log(i);
}
console.log(i); // Logs 3
```
## while
- Repeats until condition false
- Syntax similar to if

Basic:
```javascript
let i = 0;
while (i < 3) {
  console.log(i);
  i++;
}
```

Infinite loop:
```javascript
while (true) {
  console.log("Here");
}
```

## do...while
- Same as while but always executes the body of the statement at least once
- Not very common in production, but may be useful sometimes


Basic:
```javascript
let i = 0;
do {
  i += 1;
  console.log(i);
} while (i < 5);
```
Execution once even though condition false:
```javascript
do {
  console.log("Here");
} while (false);
```
Possible production use case - we want a random number that has to be over a threshold, so the body has to be executed at least once:
```javascript
// Gets a random number that's under ten
function getRandomNumberMax10() {
    return Math.floor((Math.random() * 10));
}

let randomNumber = 0;
do {
    randomNumber = getRandomNumberMax10();
    console.log(randomNumber);
} while (randomNumber < 6); // Stops the loop as soon as randomNumber is 6 or over 
```

## for...in
- Iterates over properties of object.
- Includes properties of parent objects


Basic:
```javascript
let obj = {key1: "h", key2: "sss"}
for (const i in obj) {
  console.log(`obj.${i} = ${obj[i]}`);
}
```
Works with arrays (sort of, use for of instead it's nicer)
```javascript
let obj = ["s", "d"];
for (const i in obj) {
  console.log(`obj.${i} = ${obj[i]}`);
}
```
Displays extra properties you set on arrays (arrays are objects):
```javascript
let obj = ["s", "d"];
obj.extraKey = "Sneaky"
for (const i in obj) {
  console.log(`obj.${i} = ${obj[i]}`);
}
```
Shows properties of parent objects, which may not be desired behavior. Shouldn't be an issue if it's a simple object you made yourself using {}:
```javascript
function A() {
   this.x = "I'm an own property";
}
A.prototype.y = "I'm a parent object property (Native JS Objects are strange)";

let obj = new A();
for (const i in obj) {
  console.log(`obj.${i} = ${obj[i]}`);
}
```

## for...of
- Iterates over items, not indexes
- Works with any type of iterable object not just Array (nuance you probably don't need to care about)

About as complex as it gets:
```javascript
let arr = ["s", "d"];
for (const i of arr) {
  console.log(i); // logs s and d
}
```
## forEach
- Array method that iterates through items in the array
- Basically the same as for...of but you get the element, index and the entire array given to the function on every call, which can be handy 
- Shouldn't be used to do the job of .map() (change the elements in the array to something else) or .filter() (make an array containing only some of the items)


Basic:
```javascript
let arr = ["s", "d"];
arr.forEach((element, index, array) => console.log(element, index, array))
```
Production like use case (constructing HTML by hand like this is not really that common in modern code, just the best example I could think of):
```javascript
let arr = ["s", "d"];
let str = "<ul>\n";
arr.forEach((element, index, array) => str += `  <li>${element}<li>\n`)
str += "</ul>\n";
console.log(str);
```


# Control Statements
## break
- Exit loop straight away 
- Doesn't work with forEach() (it's not a "real" loop)
- If loops are nested, exits only the loop the statement is in 
- Can use labels if you need to break out multiple layers but rarely used

Basic:
```javascript
let arr = [1, 2, 3, 4, 5];
for (const i of arr) {
  if (i == 3) {
    break;
  }
  console.log(i);
}
```
Nested loops:
```javascript
let arr = [1, 2, 3, 4, 5];
for (let i = 0; i < 3; i++) {
    for (const i of arr) {
        if (i == 3) {
            break;
        }
        console.log(i);
    }
    console.log("Looping");
}
```
## continue
- Skip the rest of the code in the statement body for that iteration of the loop


Basic:
```javascript
let arr = [1, 2, 3, 4, 5];
for (const i of arr) {
  if (i == 3) {
    continue;
  }
  console.log(i); // Skips logging 3
}
```