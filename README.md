<img width='300' src='http://10.120.129.20/io/chloe/ramda-intro/img/ramda.png'/> <h1 style="text-align:right">Ramda<h1>
---
### What's Ramda


 A Functional Programming Libraries in JavaScript
 
 - Designed in pure function style
 
 - Immutable structures
 
 - Functions are automatically curried
 
---
### Purity

A function is pure if the return value is only determined by its input values,

and does not produce side effects.

```js
const greet = (name) => 'Hello, ' + name

greet('Tracy') // 'Hello, Tracy'
```
---
### Immutability

The function creates a copy of the structure instead of changes.

```js
const arr1 =[1,2,3];

arr1.push(4);

arr1.push(5);

console.log(arr1); // [1, 2, 3, 4, 5]


var arr2 = [1, 2, 3];

console.log(R.append(4, arr2)); // [1, 2, 3, 4]

console.log(R.append(5, arr2)); // [1, 2, 3, 5]

```
---
###  Currying

A function with multiple arguments is transformed to a function with a single argument

returning another function with a single argument

```js

const add = (a, b) => a + b;

const curriedAdd = (x) => (y) => x + y;

curriedAdd(10)(5) // 15

const add2 = curriedAdd(2) // (b) => 2 + b

add2(10) // 12

```
[autoCurried](https://codepen.io/chloehsu/pen/EgaLAG?editors=0012)

---
### Why Curry

- Function can be partially configure
- Function are resue

```js
const object = [{ id: 1 }, { id: 2 }, { id: 3 }];

object.map(function(o){
    return o.id;
});

const get = R.curry(function(property, object){
    return object[property];
});

object.map(get('id')) //= [1, 2, 3]
```

---
### Installation

```
$ npm install ramda
```

```js
import R from 'ramda';
```

or from a CDN

```html
<script src="//cdnjs.cloudflare.com/ajax/libs/ramda/0.22.1/ramda.min.js"></script>
```

---
### Type Signatures

<img width="750" src='http://10.120.129.20/io/chloe/ramda-intro/img/ramda-doc.png'/>

---
### Type Signatures (cont.)

- **Named Types**:   *String*,   *Number*,   *Boolean* ...

- **List of Values**:   *[Number]*, *[String]* ...

- **Functions**:  *(Number -> Number)*,   *(String -> [String])* ...


```js
// findWords :: String -> [String]

const findWords = sentence => sentence.split(/\s+/);

findWords('She sells seashells by the seashore'); 

//=> ["She", "sells", "seashells", "by", "the", "seashore"]
```
---

### Type Signatures (cont.)

- **Currying**:  *Number → Number → Number*

```js
// calculateTax :: Number -> Number -> Number

const calculateTax = R.curry((rate,  base)
    => Math.round(100 * base + base * rate) / 100);
    
```
    
- **Type Variables** :   *(a -> b) -> [a] -> [b]*

```js
// map :: (String -> Number) -> [String] -> [Number]
map(word => word.length, ['Four', 'score', 'and', 'seven']); 
//=> [4, 5, 3, 5]

// map :: (Number -> Number) -> [Number] -> [Number]
map(n => n * n, [1, 2, 3, 4, 5]); 
//=> [1, 4, 9, 16, 25]


// map :: (a -> b) -> [a] -> [b]
```
    
---
# Let's buld a todo list
---

#### Todo list: list incomplete tasks

```js
const incomplete = function(arr) {
  
  const tamp = [];
  
  arr.map((obj)=>{
    if(obj.complete === false){
      tamp.push(obj);
    }
  });
  
  return tamp;
}

console.log(incomplete(tasks));
```
---
#### Todo list: list incomplete tasks

```js
const incomplete = function(arr) {
  
  const tamp = [];
  
  arr.map((obj)=>{
    if(obj.complete === false){
      tamp.push(obj);
    }
  });
  
  return tamp;
}

console.log(incomplete(tasks));
```

writing in ramda

```js
const incomplete = R.filter(R.whereEq({complete: false}));

console.log(incomplete(tasks));

```

---
#### Todo list: list the tasks that active by user

```js
const groupBy = function ( array , f ) {

  const groups = {};
  
  array.forEach((o)=>{
    const group = JSON.stringify(f(o));
    groups[group] = groups[group] || [];
    groups[group].push( o );  
  });
  
  return Object.keys(groups).map(( group )=>{
    return groups[group]; 
  })
}

console.log(groupBy(incomplete(tasks) , (obj) => [obj.username]));

```

---
#### Todo list: list the tasks that active by user

```js
const groupBy = function ( array , f ) {

  const groups = {};
  
  array.forEach((o)=>{
    const group = JSON.stringify(f(o));
    groups[group] = groups[group] || [];
    groups[group].push( o );  
  });
  
  return Object.keys(groups).map(( group )=>{
    return groups[group]; 
  })
}

console.log(groupBy(incomplete(tasks) , (obj) => [obj.username]));

```
writing in ramda

```js
const groupByUser = R.groupBy(R.prop('username'));
const activeByUser = R.compose(groupByUser, incomplete);

console.log(activeByUser(tasks));
```

---
### DEMO

[Todo List ](https://codepen.io/chloehsu/pen/jrPxJm?editors=0010)

[Another Curry Example](https://codepen.io/chloehsu/pen/xEGwRR)

--
### Ref

- [Ramda](http://ramdajs.com/0.22.1/index.html)
- [Why Ramda](http://fr.umio.us/why-ramda/)
- [Hands-on Functional Programming with Ramda.js](https://www.sitepoint.com/functional-programming-with-ramda/)
- [Why Curry Help](https://hughfdjackson.com/javascript/why-curry-helps/)
- [Functional Programming](https://drboolean.gitbooks.io/mostly-adequate-guide/content/)
- [Functional Programming Jargon](https://github.com/hemanth/functional-programming-jargon/tree/52b9f361b64db3c7d4b9fdc5aeeb0e92f8e33abf#type-signatures)
- [TypeSignatures](https://github.com/ramda/ramda/wiki/Type-Signatures)


