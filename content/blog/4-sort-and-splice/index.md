---
title: "Sort And Splice"
date: "2019-11-10"
description: "How to easily sort through an array of objects"
---
The other day I needed to sort through an array of objects and grab five of the objects with the highest ratings. The array looked something roughly like this (although with a lot more key value pairs):

```javascript
 const arrayOfFilms = [
  { film_title: 'The Rocky Horror Picture Show', director: 'Jim Sharman', rating: 7 },
  { film_title: 'The Mummy', director: 'Stephen Sommers', rating: 8 },
  { film_title: 'Anchorman', director: 'Adam McKay', rating: 7.5 },
  { film_title: 'Miss Congeniality', director: 'Donald Petrie', rating: 1 },
  { film_title: 'Boiling Point', director: 'Takeshi Kitano', rating: 9 },
  { film_title: 'The Lobster', director: 'Yorgos Lanthimos', rating: 8 },
  { film_title: 'Avengers: Endgame', director: 'Joe Russo', rating: 3 }
 ];
```

Sorting by film rating is easy using `Array.prototype.sort()`:

```javascript
const sortedArrayOfFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating);
```

The `sortedArrayOfFilms` should look like:

```javascript
 const sortedArrayOfFilms = [
  { film_title: 'Boiling Point', director: 'Takeshi Kitano', rating: 9 },
  { film_title: 'The Mummy', director: 'Stephen Sommers', rating: 8 },
  { film_title: 'The Lobster', director: 'Yorgos Lanthimos', rating: 8 },
  { film_title: 'Anchorman', director: 'Adam McKay', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', director: 'Jim Sharman', rating: 7 },
  { film_title: 'Avengers: Endgame', director: 'Joe Russo', rating: 3 },
  { film_title: 'Miss Congeniality', director: 'Donald Petrie', rating: 1 }
 ];
```

But I only needed to grab the top five! To do this we can use `Array.prototype.splice()`:

```javascript
const topFiveFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating).splice(0,5);
```

This should give us:

```javascript
 const topFiveFilms = [
  { film_title: 'Boiling Point', director: 'Takeshi Kitano', rating: 9 },
  { film_title: 'The Mummy', director: 'Stephen Sommers', rating: 8 },
  { film_title: 'The Lobster', director: 'Yorgos Lanthimos', rating: 8 },
  { film_title: 'Anchorman', director: 'Adam McKay', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', director: 'Jim Sharman', rating: 7 }
 ];
```

But what if we don't have five objects in the array? What if there is only two, or three?
Not a problem - let's say we have the following array of objects:

```javascript
const arrayOfFilms = [
  { film_title: 'The Rocky Horror Picture Show', director: 'Jim Sharman', rating: 7 },
  { film_title: 'Boiling Point', director: 'Takeshi Kitano', rating: 9 },
  { film_title: 'Anchorman', director: 'Adam McKay', rating: 7.5 }
];
```

We can use the same sort and splice methods, and using `.splice(0,5)` shouldn't throw an error even if we only have two or three objects in the array:

```javascript
const topFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating).splice(0,5);
```

Using the above, we should end up with:

```javascript
const topFilms = [
  { film_title: 'Boiling Point', director: 'Takeshi Kitano', rating: 9 },
  { film_title: 'Anchorman', director: 'Adam McKay', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', director: 'Jim Sharman', rating: 7 }
];
```
