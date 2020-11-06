---
title: "Adding A Key:Value Pair With Array.prototype.map()"
date: "2019-10-22"
description: "How to add a key:value pair to an object with ES6"
---

Let's say you have the following array of objects:

```javascript
 const arrayOfActors = [
  { first_name: 'Bill', last_name: 'Murray' },
  { first_name: 'Michael', last_name: 'Fassbender' }
 ];
```

and you need to add a new key:value pair, e.g. profession, to all elements in the array, you can use
`Array.prototype.map()` to map over every element in the array and create a new array with the provided key:value pair:

```javascript
const newArrayOfActors = arrayOfActors.map(a => (a.profession = 'actor'));
```

The array `newArrayOfActors` should now contain:

```javascript
[
  { first_name: 'Bill', last_name: 'Murray', profession: 'actor' },
  { first_name: 'Michael', last_name: 'Fassbender', profession: 'actor' }
]
```
