# Desctructuring

basic ones are:
```javascript
let a, b;
[a, b] = [10, 20]
console.log(a) // 10
console.log(b) // 20

[a, b, ...rest] = [1,2,3,4,5,6,7,8,9]
console.log(a) // 1
console.log(b) // 2
console.log(rest) // [3,4,5,6,7,8,9]

const x = [1,2,3,4,5]
const [y,z] = x
console.log(y) // 1
console.log(z) // 2

```

**question**
what is the aim of using arrays like that `var myArr = [a=10, b=20]`  
when we do this, js creates two new variables a and b and we can reach them directly, but myArr got only values 10 and 20

we can use default variables and desctructuring together
```javascript
[a=5, b=7] = [1]
console.log(a) // 1
console.log(b) // 7
```

also we can use destructuring in objects
```javascript
const o = {p:42, q: true}
const {p, q} = o
console.log(p) // 42
console.log(q) // true
```

but above code we can use other variables, something like below won't work
```javascript
const o = {p:42, q:true}
const {x, y} = o
console.log(x) // undefined
console.log(y) // undefined
```

another important area of usage of desctructuring is unpacking fields from objects passed as function parameter
```javascript
const user = {
    id: 42,
    displayedName: 'john42',
    fullName: {
        firstName: 'john',
        lastName: 'doe'
    }
};

function userId({id}) {
    return id;
}

function whois({displayedName, fullName: {lastName: name}}) {
    return `${displayedName} is ${name}`
}

console.log(userId(user)) // 42
console.log(whois(user)) // john42 is doe
```

**important:** if we gonna use ${variable} in return, we need to use ´ symbol not ' or " symbol.

# Arrow functions

Two important things about arrow functions are, first arrow functions don't have proptotype property like normal functions.
```javascript
var foo = function() {
    return 'hello'
}
foo.prototype // returns prototype of a function

var moo = () => {
    return 'hello'
}
moo.prototype // undefined
```

another one is, arrow functions don't have their this variable. All other function objects (functions are actually objects) have their own this, but not arrow functions. Arrow functions looks the execution environment which they in and use this of that environment

```javascript
// first if you use variable without this keyword in function constructor than this variable goes to global scope
function Person(firstName, lastName) {
  this.firstname = firstName;
  this.lastname = lastName;
  this.getFullName = function() {
    return this.firstname + ' ' + this.lastname;
  }

  changeName = function() {
    this.firstname = 'aaaaa';
  }
}
// here changeName we can find with
// window.changeName

// second, if we use normal function like this
function Person2(firstName, lastName) {
  this.firstname = firstName,
  this.lastname = lastName,
  this.age = 0
  setInterval(function growUp() {
    this.age ++;
  }, 1000)
}
// when growUp function executed, it has it's own this keyword and this references the global scope, because growUp execuded in global scope, thats why this.age in growUP won't same with age in Person2
```

but arrow functions doesn't have their own this keyword, they look and use the this in their lexical scope. Arrow functions first search this keyword in their current scope, and if they can't find arrow functions will look their enclosing scope

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the Person object
  }, 1000);
}

var p = new Person();
```
above code works, because the arrow function doesn't have it's own this, so it looks the lexical scope which is scope of Person and use the this value of person, by this way arrow function can reach the age attribute.

It is not a good idea using arrow functions as a object method, because this keyword in arrow function will show the enclosing scope not the object scope.  The reasoin is when we create object we are not actually creating a new scope.

```javascript
var obj = { // does not create a new scope
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log(this.i, this);
  }
}

obj.b(); // prints undefined, Window {...} (or the global object)
obj.c(); // prints 10, Object {...}
```
