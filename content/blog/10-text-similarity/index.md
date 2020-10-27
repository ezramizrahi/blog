---
title: "Text Similarity and Set Theory"
date: "2020-10-27"
description: "Finding common elements between different text sources"
---
The other day I decided to make an application that returns the words shared between two texts (e.g. if two different paragraphs contain the word "intersection", then the application should render the word "intersection"). This was also a good chance to go over set theory - specifically the concept of intersection.

In set theory, the intersection of two sets A and B is the set containing all elements of set A that also belong to set B. This can be written as:

![Equation](equation.svg)

A practical example of this is would be looking at two different texts A and B as sets, and the intersection between those texts are all of the words in text A that also belong to text B.

To get the intersection between the two sets of text, I first create an array of words for each text set. I then create a new JavaScript Set, and push the contents of the above array into the Set - this conveniently removes any duplicates. Next, I create a new Set that only has words from text A that also belong to text B. The full code is below:

```javascript
const [textInputOne, setTextInputOne] = useState('');
const [textInputTwo, setTextInputTwo] = useState('');
const [textArrayOne, setTextArrayOne] = useState([]);
const [textArrayTwo, setTextArrayTwo] = useState([]);

const handleTextInputOne = (event) => {
  setTextInputOne(event.target.value);
};
const handleTextInputTwo = (event) => {
  setTextInputTwo(event.target.value);
};

useEffect(() => {
  const arrayOfWords = textInputOne.match(/\b(\w+)\b/g);
  setTextArrayOne(arrayOfWords);
}, [textInputOne]);
useEffect(() => {
  const arrayOfWords = textInputTwo.match(/\b(\w+)\b/g);
  setTextArrayTwo(arrayOfWords);
}, [textInputTwo]);

// Set Intersection
let setOne = new Set();
let setTwo = new Set();
textArrayOne?.forEach(t => setOne.add(t));
textArrayTwo?.forEach(t => setTwo.add(t));
let intersection = new Set([...setOne].filter(x => setTwo.has(x)));
```

The code could be a bit more DRY, but it works fine as is.

Check out the code on [GitHub](https://github.com/ezramizrahi/text_similarity), and try the live version over at [https://text-similarity.netlify.app/](https://text-similarity.netlify.app/).
