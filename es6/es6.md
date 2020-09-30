---
title: ES6 Essentials
---
## Variable Definitions (Let, Var, Const)

You can define variables using three different keyword types in ES6, these are :

* const (default) - use if you don't need to update the variable
* let - use if you need to update the variable
* var - use only if you need to use the variable outside of the block (for loops)

```js
const anArray = [ 1,2,3 ]; //I cannot be changed. Data can be pushed or unshifted to me
let aString = "hello"; //I can be updated

const anObject = {
	anArray:[ 1,2,3 ],
	aString:'hello'
}

anObject.anArray = [ 2,6,3 ] //objects can be updated even if const like so
anObject.aString = 'bye'
```

## Modulus Operator
The modulus operator returns the remaining number of a devision equation.
```js
const sum = (39 / 4) //9.75
const num = (39 % 4) //3

// 4 goes into 39 a total of 9 times.
// 4 * 9 = 36. what about the remainder?
// 4 * 0.75 = 3 <- dere ya go
```

### Fizzbuzz
A simple application that makes use of the modulus operator
```js
for (var i = 1; i <= 10; i++) {
    var f = i % 3, b = i % 5;
    console.log("f:" + f, "b:" + b, i == 10 ? "FizB" : f == 0 ? "Fiz" : b == 0 ? "Buz" : i);
}
```
```txt
f:1 b:1 1
f:2 b:2 2
f:0 b:3 Fiz
f:1 b:4 4
f:2 b:0 Buz
f:0 b:1 Fiz
f:1 b:2 7
f:2 b:3 8
f:0 b:4 Fiz
f:1 b:0 FizB
```
WTF is going on here? `var f = i % 3` equals the remainder of (iteration / 3) right? yes. on iteration 3 notice f = 0 because 3/3 has no remainder and hence the Fiz. Likewise on iteration 5 `b = 0` and thus Buz because (iteration 5 / 5 ) has no remainder. Ok what about the console log rubbish? This will help:

```js
const foo = 3
console.log(foo == 1 ? 'one' : foo == 2 ? 'two' : 'fak knows') //fak knows
```
if `foo == 1` c.log 'one' else if `foo == 2` c.log 'two' else 'fak knows'

## Iterating With Loops
There are several different types of loops. Such as for, while, forEach and so on.
Here are a couple examples :

### For Loop
The most common loop. Can be used for looping through pretty much anything
```js
for (let i = 1; i <= 5; i++){ //incremental for loop
    if ( i === 3 ) {
        continue; //will skip 3rd iteration and move on to 4th
    }
    console.log('ha: ' + i);
}

for (let i = 1; i <= 5; i++){ //incremental for loop
    console.log(i);
    if ( i === 3 ) {
        break; //will stop loop once iteration gets beyond three
    }
}

for (let i = 5; i > 0; i--){ //decremental for loop
    console.log(i);
}

const args = ['foo', 'bar', 'another']

for ( let i = 0; i < args.length; i++ ) {
    console.log(args[i]) //returns foo bar another
}
```

### For Of Loop
For of loops are useful for looping through arrays
```js
const args = ['foo', 'bar', 'another']

for (let arg of args) { //loops through array like above
    console.log(arg)
}

```

### For In Loop
This is used for looping through objects
```js
const obj = {
    firstname:'john',
    lastname:'doe'
}

for ( let i in obj ) {
    console.log(obj[i])
}

for (const [key, value] of Object.entries(obj)) {
    console.log(key + ":" + value);
}
```

### While and Do While Loop
The purpose of these loops is to execute a statement or code block repeatedly as long as an expression is true.

```js
let i = 1
while ( i <= 10 ) {
    console.log(i) //returns 1 to 10
    i++
}

let x = 1
do { //does iteration first then looks at condition after
    console.log('do: ' + x)
    x++
} while ( x < 0 ) //executes once then realizes

```

### For Each Loop
Used to loop through array
```js
const mearr = ['foo', 'bar']

mearr.forEach(me => {
    console.log(me)
});

const obj = {
    firstname:'john',
    lastname:'doe'
}

Object.entries(obj).forEach( //using foreach on an object
    ([key, value]) => console.log(key + " : " + value)
);
```

## Block Scope
Scopes exist meaning that variables defined within functions are only accessible within that function and not outside (unless passed or pushed)
If doing a for loop use let instead of var. var leaks out the variable to the next block. In this example `i` is accessible in the outer function. This is not good as we shouldn't be able to access the last iteration

```js
const foo = () => {
    const foof = (n) => {
        return n ** 2;
    }
    for(var i=1; i<=10; i++){
        console.log("foof: " + i + " " + foof(i));
    }
    console.log(i) //is accessible ( bad )
}

const bar = () => {
    const barf = (n) => {
        return n ** 2;
    }
    for(let i=1; i<=10; i++){ //use let to fix
        console.log("barf: " + i + " " + barf(i));
    }
    console.log(i) //not accessible ( good )
}

foo();
bar();
```

## Arrow Functions
Arrow functions are simply javascript functions. These functions however make use of the es6 new syntax. The new syntax is very similar as seen below :
Note the keyword `this` cannot be used within arrow functions thus arrow functions are only best choice when working with closures or callbacks, but not a good choice when working with class/object methods or constructors.

```js
//old js function
const getRandomNumber = function(limit) {
    let random = Math.random() * limit;
    return Math.ceil(random);
}

//new js function
const getRandomNumber = (limit) => {
    let random = Math.random() * limit;
    return Math.ceil(random);
}

//one liner
const getRandomNumber = (limit) => Math.ceil(Math.random() * limit);
```

## Default Values
In the example below you have a function that displays three names and `hello` is perpended to each name like so :

```js
const sayHi = (name1, name2, name3, greeting) => {
    console.log(greeting + " " + name1);
    console.log(greeting + " " + name2);
    console.log(greeting + " " + name3);
}

sayHi('Bill', 'John', 'David', 'Hello'); //If hello missing an error occurs

const sayHi = (name1, name2, name3, greeting = 'Hi') => {
    console.log(greeting + " " + name1);
    console.log(greeting + " " + name2);
    console.log(greeting + " " + name3);
}

sayHi('Bill', 'John', 'David'); //Hello is missing no error instead because hi is default

```

## Rest Parameters
The rest parameter allows us to pass an indefinite number of parameters to a function.
In the example below `...names` is a rest parameter. This means that an unlimited amount of names could be passed into the `sayHi` function. Rest parameters must be assigned last in the function otherwise it won't work. `Hello` is inputed as the `greeting` variable.

```js
const sayHi = (greeting, ...names) => {
    names.forEach(name => {
        console.log(greeting + " " + name);
        console.log(names[1]); //prints John
    });
}
sayHi('Hello', 'Bill', 'John', 'David', 'Fred');

```

Here is another example :
```js
function sum(...args) {
    let total = 0;
    args.forEach((arg) => {
        return total += arg
    });
    console.log(total)
}

const arr = [1,2,3]
sum(...arr); //returns 6
```

## Spread Operator
The Spread Operator is unpacking collected elements such as arrays into single elements. Almost like the opposite to rest parameters. Uses the same syntax method.

```js
console.log([arr, 4,5,6]) //without spread operator returns [ [ 1, 2, 3 ], 4, 5, 6 ]
console.log([...arr, 4,5,6]) //with spread operator returns [ 1, 2, 3, 4, 5, 6 ]
```

## Template Literals
Template literals are another way of creating strings. They come in handing when working with multiple variables. Template literals make use of the back tick keys.

```js
const foo = `this is a normal`
const bar = `string`
console.log( `${foo} ${bar}` ) //returns this is a normal string

//Can write javascript within the ${}
console.log(`100 over 5 = ${ 100/5 }`) //returns 100 over 5 = 20
```

## Tagged Template Literals
Tagged template literals allow you to parse template literals in whatever way you want. It works by combining functions with template literals.
Like rest parameters the variables should be passed into the functions last and the string needs to be passed in first. `string` is passed in first which is actually an array which you can access like `string[2]`
You don't call tagged templates like regular functions (`greet1()`) you instead need to remove the brackets and use the back ticks

```js
const greet1 = (string, ...names) => { //With rest params
    //string = [ 'hello ', '', ' nice to meet you' ]
    //names = [ 'sir', 'bar' ]
    const now = new Date()
    const time = now.getHours() < 12 ? 'Day' : now.getHours() < 17 ? 'Night' : 'Afternoon';
    return `Good ${time} ${names[1]}${string[2]}`;
    //Good Afternoon bar nice to meet you
}

const greet2 = (string, fname, sname) => { //Without rest params
    //string = [ 'hello ', '', ' nice to meet you' ]
    //fname = sir
    //sname = bar
    return `${string[0]}${fname} ${sname}`
    //hello sir bar
}

const fname = 'sir';
const sname = 'bar';

//here is the tagged template literal
const foo = greet1 `hello ${fname}${sname} nice to meet you`;
console.log(foo); //greet1

//here is the tagged template literal - greet2
console.log(greet2 `hello ${fname}${sname} nice to meet you`);
```

## Extended Literals
This topic has to do with Unicode characters and other special character sets.

```js
const foo = Number(404).toString(2)

console.log(foo) //returns 110010100
console.log(0b110010100 === 404) //returns true
```

## Objects
This topic just discusses javascript objects. Objects can have variables and functions within them.


```js
const username = 'Michael Scott'
const password = 'bigboobz'
const params = 'isAdmin'
const newPswd = 'bigBoobz69'

const auth = {
    u:username, //uses u as the variable name
    p:password,
    foo: (newPassword) => {
        return newPassword + "salt";
    }
}

console.log(auth) //{ u: 'Barry', p: 'bigboobz', foo: [Function: foo] }
console.log(auth.foo(newPswd)) //returns bigBoobz69salt

const auth2 = {
    username, //uses the variable as the name
    password,
    [params]:true,
}

console.log(auth2) //{ username: 'Barry', password: 'bigboobz', isAdmin: true }
```

## Destructing Arrays and Objects
Destructing refers to extracting properties from arrays and objects into separate variables.

### Destructing Arrays
Here is what destructing arrays looks like. `[const a,b,,c]..` is the destructing. This creates assigns the array items in `languages` to new separate variables.

```js
const languages = ['JS', 'PHP', 'GO', 'BASH']
const [a,b,,c,d = 'fallback'] = languages //,, skips go. d is assigned value if non-existent

console.log(a) //returns JS
console.log(b) //returns PHP
console.log(c) //returns BASH
console.log(d) //returns fallback

```

### Destructing Objects
Here is what destructing objects looks like. This is pretty much the same. Like before you can assign fallbacks if the variable does not exist in the array or object

```js
const auth = {
    user: 'James',
    pass: 'semaj',
    isAdmin: true
}

const { user, isAdmin, unknown = 'fallback' } = auth

console.log(user) //returns James
console.log(isAdmin) //returns true
console.log(unknown) //returns fallback

const { pass, ...daRest } = auth //notice pass is not included in daRest

console.log( daRest ) //returns { username: 'James', admin: true } notice pass not there
```

### Parameter Context Matching
using parameter context matching we can use variables from objects directly within a function by passing them through the function via `({ var1, var2 ... })`

```js
const auth = {
    user: 'James',
    pass: 'semaj',
    isAdmin: true
}

const login1 = ( info ) => { //traditional way
    return info.user === 'James' && info.isAdmin === true
}

const login = ({ user, isAdmin }) => { //using parameter context matching
    return user === 'James' && isAdmin === true //no need to info.var
}

console.log( login1( auth ) ) //returns true
console.log( login( auth ) ) //returns true
```

## Modules
Modules allow you to separate your code into other files. You can then import the file you need to access the external functions or variables

#### secondary.js :
```js
const foo = "hello"

const foofunc = (a, b, foo) => {
    return `${a}, ${b}, ${foo}`
}

export {
    foo,
    foofunc
}
```

#### index.js :
```js
import { foofunc, foo } from './secondary.js'

console.log(foofunc('hi', 2, foo)) //returns hi, 2, hello
```

### Using Modules With NodeJS
For some reason this doesn't work 'out-of-the-box'. You need to specifically tell node to use modules.

#### package.json :
```js
{
  "name": "es",
  "version": "1.0.0",
  "description": "",
  "type":"module", //add this to use modules
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}
```

### Using Modules With Web
Again this doesn't 'just work' so add the module parameter to a script tag.

#### index.html :
```html
<script type="module" src="index.js"></script>
```

## Classes
Classes are useful when you need to have many objects with similar variables. For example :

```js
const obj = {
    id:19113,
    first_name:'sam',
    last_name:'sharp',
    getID() {
        return "id : " + this.id
    }
}

console.log(obj.first_name) //returns sam
console.log(obj.getID()) //returns 19113
```

This is great but what if we need many of these objects because for example we had many users? We can create classes as seen below.
when you call `new User` the constructor runs and creates a new object

 ```js
class User {
    constructor(id, first_name, last_name) {
        this.uid = id;
        this.ufirst_name = first_name;
        this.ulast_name = last_name;
    }
    getID(){ //this is called a method
        return this.id //User { uid: 19113, ufirst_name: 'sam', ulast_name: 'sharp' }
    }
}

const sam = new User(19113, 'sam', 'sharp')

console.log(sam.ufirst_name)
console.log(sam.getID())
```

### Method Chaining
Method chaining allows you to call multiple methods on one line.

```js
class User {
    constructor(id, first_name, last_name) {
        this.uid = id;
        this.ufirst_name = first_name;
        this.ulast_name = last_name;
    }
    getID(){
        console.log(this.uid)
        return this //must return this for getID to be chainable
    }
    updateName(){
        return this.ufirst_name + "foo"
    }
}

const sam = new User(19113, 'sam', 'sharp')

//without chaining
console.log(sam.getID())
console.log(sam.updateName())

console.log(sam.getID().updateName()); //with chaining
```

Returning `this` makes the method chainable. Why? Well `return this` simply returns the object. Think about it `this.id` returns the id in the object so `return this` must return the whole object. You can then call a method as usual thus `.updateName()` works as it is just like calling the method on the object.

### Getters and Setters
Pretty self explanatory gets and sets. The setter and getter can be named the same. These are just like creating methods however using getters and setters mean you don't need to use parentheses when calling.

```js
class User {
    constructor(id, first_name, last_name) {
        this.uid = id;
        this.ufirst_name = first_name;
        this.ulast_name = last_name;
    }
    set meid(n){ //setter
        this.uid += n
    }
    get meid(){ //getter
        return this.uid
    }
}

const sam = new User(19113, 'sam', 'sharp')

sam.meid = 5 //how you call the setter
console.log(sam.meid) //gets the new value
```

### Static Methods
Sometimes you may need to reference a method from a class without an object being created. Using the keyword `static` before the method resolves this issue.
Using the `static` keyword the method can now be called by `User.()` instead of the instance e.g. `sam.()`. Calling the static method with a parameter like `+ this._uid` will simply return undefined.
Note calling `User.()` can only access the static methods and thus calling `sam.()` can only access the non-static methods
```js
class User {
    constructor(name, age) {
        this._uid = (Math.random()*99).toFixed(0);
        this._name = name;
        this._age = age;
    }
    static getProfile(){
        return 'http://foo.bar/';
    }
    getUserProfile(){
        return 'http://foo.bar/uid/'+this._uid;
    }
}

const sam = new User('sam sharp', 12, ['php', 'js', 'bash'])

console.log('sam.() : ' + sam.getUserProfile()) //returns sam.() : http://foo.bar/uid/78
console.log('User.() : ' + User.getProfile()) //returns User.() : http://foo.bar/
```

### Class Inheritance
Class inheritance literally refers to classes inheriting methods or variables from other classes.
A class needs to `extends` another class. Once that has been implied then the new class `Developer` in this case can now access all methods and variables within the `User` class
```js
class User {
    constructor(name, age) {
        this._uid = (Math.random()*99).toFixed(0);
        this._name = name;
        this._age = age;
    }
    getProfile(){
        return 'http://foo.bar/'+this._uid;
    }
}

class Developer extends User{
    constructor(name, age, languages ){
        super(name, age);
        this._languages = languages;
    }
}

const sam = new Developer('sam sharp', 12, ['php', 'js'])
console.log(sam)
//Developer { _uid: '56', _name: 'sam sharp', _age: 12, _languages: [ 'php', 'js' ] }
console.log(sam.getProfile()) //returns http://foo.bar/56
```
