---
title: "Pokémon, Async Await, and React Hooks"
date: "2020-11-02"
description: "Using Async Await patterns and React Hooks to create a Pokémon dashboard"
---
I was recently inspired to make a dashboard, and what better content to use than Pokémon? This also gave me the chance to demonstrate the use of `async await` patterns as well as React Hooks. Below I'll walk through some of the code I used to create [this Pokémon dashboard (live on Netlify)](https://pokemon-dashboard.netlify.app/).

### Feed.js

The `Feed.js` card contains a feed (basically a list) of recent activity. In this case, Pokémon recently captured, or Pokémon that have recently evolved.

First, I declare my state using the `useState` Hook:
```
const [pokemonData, setPokemonData] = useState([]);
```

`pokemonData` will contain Pokémon data fetched from [PokéApi - The RESTful Pokémon API](https://pokeapi.co/). Its initial state starts off as an empty array.

Next, I use another React Hook - `useEffect`. The `useEffect` Hook is useful for performing actions like fetching data.

Inside of the `useEffect` Hook I declare an `async` function - `getPokemon`. Inside of this, I fetch data asynchronously using the `await` keyword. First, I fetch a list of Pokémon from `https://pokeapi.co/api/v2/pokemon?limit=5&offset=6` (this list contains a Pokémon name and a url that allows you to fetch more detailed information on a specific Pokémon) and store it inside of the `response` variable. Next, I map over this response, using each url to make another asynchronous request, fetching detailed data for each Pokémon. This last step uses the `Promise.all()` method - this method ["is typically used when there are multiple related asynchronous tasks that the overall code relies on to work successfully — all of whom we want to fulfill before the code execution continues"](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all). In this case, I have five asynchronous tasks (as I'm fetching details on five Pokémon) that I'm relying on for the `Feed.js` card to work successfully.

On the successful return of those `Promises`, I then update the state using `setPokemonData(myPokemon)`. I can now map the relevant Pokémon data in the `Feed.js` card, or pass that data to any child components like the `CatchLocation.js` component, where I display where a Pokémon was caught.

```javascript
const [pokemonData, setPokemonData] = useState([]);

useEffect(() => {
  const getPokemon = async () => {
    try {
      let myPokemon = [];
      const response = await axios.get("https://pokeapi.co/api/v2/pokemon?limit=5&offset=6");
      let urls = response.data.results;
      myPokemon = await Promise.all(urls.map(async(url) => {
        return await axios.get(url.url);
      }));
      setPokemonData(myPokemon);
    } catch (err) {
      console.log(err);
    }
  };
  getPokemon();
}, [])
```

Check out the code on [GitHub](https://github.com/ezramizrahi/pokemon_dashboard).
