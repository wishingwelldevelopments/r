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
    const time = now.getHours() < 12 ? 'Day' : now.getHours < 17 ? 'Night' : 'Afternoon';
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

## Extended Object Literals
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
