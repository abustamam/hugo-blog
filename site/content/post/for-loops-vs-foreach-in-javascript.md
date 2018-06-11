
+++
date = "2017-10-11T15:54:29"
draft = false
title = """For loops vs forEach in JavaScript"""
slug = "for-loops-vs-foreach-in-javascript"
tags = []
banner = ""
aliases = ['/for-loops-vs-foreach-in-javascript/']
+++

![forvsforeach](/images/2017/10/forvsforeach.jpg)
Fight!

*Also covered: for ... of and for ... in*
## Loops in JavaScript: More than one way to skin it
JavaScript is a curious beast. There are always a ton of different ways to do simple things, and it seems like every programmer has his/her own way of doing it. This article will cover one such debate.

### The for loop: learning to count again
The for loop is likely the first type of loop you learned about when you were learning JavaScript. In the off chance you aren’t familiar, it looks like this:

```js
for (var i = 0; i < 10; i++) {
  console.log(i);
}
// or, if you want to reduce variable initializations:
var i;
for (i = 0; i < 10; i++) {
  console.log(i);
}
```

This is a common way that people like to iterate through arrays. Take this array for example:

```js
var students = [
  {
    name: 'Coolio',
    age: 20,
    grade: 'A+'
  },
  {
    name: 'Mark',
    age: 45,
    grade: 'C+'
  }
];
```

You will often see this array get traversed like so:

```js
for (var i = 0; i < students.length; i++) {
  console.log(students[i]);
}
```
This is a perfectly valid way of traversing an array. You can even do nested arrays:
```js
for (var i = 0; i < arr.length; i++) {
  for (var j = 0; j < arr[i].length; j++) {
    console.log(arr[i][j]);
  }
}
```
So far so good. It doesn’t get much simpler than that now does it?
Actually, it does.

### A challenger appears: forEach

`forEach` is a method on the Array prototype. In other words, you can call it on any array, like so:
```js
students.forEach(function(student) {
  console.log(student);
});
```
Hey, that almost looks like English! Go through each item in the students array and execute this function, which logs each student. I don’t even need to know how to count anymore! See ya for loop!
“But what if I want the iteration number too?”

Worry not, my friend! The callback function actually accepts three arguments:

- element value
- element index
- array being traversed (I have never used this)

So these would be functionally equivalent:

```js
for (var i = 0; i < arr.length; i++) {
  console.log('Element', i, 'is', arr[i]);
}
arr.forEach(function(element, i) {
  console.log('Element', i, 'is', element);
});
```

### When to use a for loop over forEach

`forEach` comes with a little caveat. It only works on arrays. So if you want to actually count, `forEach` probably isn’t your friend.

It may be worth noting that in my two years of professional web development, I have never used a for loop, mainly because the use cases of a “counter” are actually quite limited.

You may have come across this article regarding performance costs to forEach. If that is a concern to you, there are two options:

1. Use lodash. I use lodash in almost all my projects. In case you don’t know what lodash is, it’s basically a utility library in JavaScript that lets you do a lot of common things. lodash has a lot of helpful iteration methods, such as forEach and map. These also work on objects, which is really neat. Usage might look like this:

```js
_.forEach(arr, function(element, i) {
  console.log(element);
}); 
```

2. Create a helper function. lodash actually uses a while loop under the covers, so it ends up being something like this:

```js
function newForEach(arr, callback) {
  let i = 0;
  while (i < arr.length) {
    callback(arr[i]);
    i++;
  }
}
```

Remember, we don’t write code for computers — we write code for humans. Since forEach is pretty idiomatic in many languages, it becomes extremely explicit what you’re doing when you use a forEach method. Regular for loops are not bad, I just think forEach reads better and makes it simpler to reason about.

### Bonus: what about `for…in` and `for…of`?
First, for...in. I see a lot of people use `for...in` in a lot of cases. Sometimes even in array iteration, even though Mozilla says not to.

> Note: `for...in` should not be used to iterate over an Array where the index order is important.

From https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in

The thing I don’t like about for...in is that it is very unintuitive! Take this for example:

```js
var arr = [
  'Mike', 
  'Steven'
];

for (var i in arr) {
  console.log(i);
}
```

What do you think will be console logged? If you say a name, you’re absolutely wrong, and that’s fine — because it’s completely unintuitive.
`for...in` actually iterates through the enumerable properties of an object/array. That means the properties that exist in the array, so the X that goes in object[X] or array[X]. For arrays, that’s the index. For objects, they’re the keys (not the values).

So essentially, it’s a for loop that also works with objects. However, the problem with using it to work with objects is that if the object inherits anything from another object, or has methods on its prototypes, those will be included as well. Many times, you don’t want those to be included, so you’d have to do hasOwnProperty(X).

If you want to iterate through objects, I’d suggest using 

```js
Object.keys().forEach(function(key) {
  // stuff
});
```

...or, you could use `for...of`!

`for...of` iterates on the other side of iterables: so instead of iterating through the keys it iterates through the values.

```js
var arr = [
  'Mike', 
  'Steven'
];

for (var person of arr) {
  console.log(person);
}
```

This does exactly what you expect it to — console logs the person! It also iterates through objects, strings… basically any sort of collection of “things.”

Is there a problem with this? If you only care about the value, then for...of will work perfectly for you. However, if you also care about the key or index, then for...of will not give that to you. You could use for...in and simply get the value by using a “getter” (that is, arr[i]), but then you’re back in square one with for loops.

So, to summarize:

- Arrays: use for...of if you don’t care about index value. I like to use forEach for all my iterations though — no refactoring needed if I ever decide I do need the index value.
- Objects: use for...of if you don’t care about key value; Object.keys() if you do (and use obj[key] to get the value).
- Everything: use lodash if it is a possibility. There’s a reason it’s the most downloaded and depended-on library on NPM.

Hope you enjoyed this little write-up!

