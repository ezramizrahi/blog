---
title: "Sort and Splice"
date: "2019-11-10"
---
The other day I needed to sort through an array of objects and grab five of the objects with the highest ratings. The array looked something roughly like this (although with a lot more key value pairs):

```
 const arrayOfFilms = [
  { film_title: 'The Rocky Horror Picture Show', rating: 7 },
  { film_title: 'The Mummy', rating: 8 },
  { film_title: 'Anchorman', rating: 7.5 },
  { film_title: 'Miss Congeniality', rating: 1 },
  { film_title: 'Boiling Point', rating: 9 },
  { film_title: 'The Lobster', rating: 8 },
  { film_title: 'Avengers: Endgame', rating: 3 }
 ];
```

Sorting by film rating is easy using `Array.prototype.sort()`:

```
const sortedArrayOfFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating);
```

The `sortedArrayOfFilms` should look like:

```
 const sortedArrayOfFilms = [
  { film_title: 'Boiling Point', rating: 9 },
  { film_title: 'The Mummy', rating: 8 },
  { film_title: 'The Lobster', rating: 8 },
  { film_title: 'Anchorman', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', rating: 7 },
  { film_title: 'Avengers: Endgame', rating: 3 },
  { film_title: 'Miss Congeniality', rating: 1 }
 ];
```

But I also needed to only grab the top five! To do this we can use `Array.prototype.splice()`:

```
const topFiveFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating).splice(0,5);
```

This should give us:

```
 const topFiveFilms = [
  { film_title: 'Boiling Point', rating: 9 },
  { film_title: 'The Mummy', rating: 8 },
  { film_title: 'The Lobster', rating: 8 },
  { film_title: 'Anchorman', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', rating: 7 }
 ];
```

But what if we don't have five objects in the array? What if there is only two, or three?
Not a problem - let's say we have the following array of objects:

```
const arrayOfFilms = [
  { film_title: 'The Rocky Horror Picture Show', rating: 7 },
  { film_title: 'The Mummy', rating: 8 },
  { film_title: 'Anchorman', rating: 7.5 }
];
```

And after we again use the following sort and splice methods:

```
const topFiveFilms = arrayOfFilms.sort((a, b) => b.rating - a.rating).splice(0,5);
```

We should end up with:

```
const topFiveFilms = [
  { film_title: 'The Mummy', rating: 8 },
  { film_title: 'Anchorman', rating: 7.5 },
  { film_title: 'The Rocky Horror Picture Show', rating: 7 }
];
```
