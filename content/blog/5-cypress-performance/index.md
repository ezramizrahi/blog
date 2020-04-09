---
title: "Site Performance With Cypress, Plotly, And The Performance Interface - Part 1"
date: "2020-04-09"
description: "Use Cypress, Plotly, and the Performance Interface to measure and visualize site performance"
---
If you haven't heard of it, [Cypress](https://www.cypress.io/) is a fantastic E2E JavaScript testing framework. It's straightforward to both set up and use. An interesting feature of Cypress that I've been exploring recently, is that Cypress can also be used to grab site/app performance metrics.

This post will take a quick look at one way you could go about grabbing performance data during a test using Cypress and the [Performance Interface](https://developer.mozilla.org/en-US/docs/Web/API/Performance). Then, I'll quickly go over one way that you could visualize that data using [Plotly](https://plotly.com/python/). This post also assumes a bit of familiarity with Cypress, Plotly, and the Performance Interface.

For this example, let's say you have an app that calls the API `/api/1.0/films` when you visit the route `/films`. The data returned from this call could be a list of a user's favourite films, a total list of films in the database, whatever really. The data from this call could be used to populate a table on the page with film title, image, and so on. We want to know how long it takes for this specific resource to load so we can assess its performance, and maybe improve it for a better end-user experience.

To get this data, we just need to access the [window.performance](https://developer.mozilla.org/en-US/docs/Web/API/Window/performance) object after we've confirmed our call to `/api/1.0/films` has been successful.

The `window.performance` object contains performance information for the current page, and exposes several performance related APIs. What we're after are performance entries for XMLHttpRequests. So, let's grab that object, get all entries of the type `resource`, and then grab only those entries which match the initiatorType `xmlhttprequest`.

```javascript
cy.window().then((win) => {
  // get all xmlhttprequest resources
  const xmlHttpRequests = win.performance.getEntriesByType('resource').filter((x) => x.initiatorType === 'xmlhttprequest');
});
```

The above should return an array of objects that looks something like:

```javascript
  [
    { initiatorType: 'xmlhttprequest', ... },
    { initiatorType: 'xmlhttprequest', ... },
    ...
  ]
```

Let's filter even more - we only want performance metrics for `/api/1.0/films`:
```javascript
cy.window().then((win) => {
  const substring = '/api/1.0/films';

  // get all xmlhttprequest resources
  const xmlHttpRequests = win.performance.getEntriesByType('resource').filter((x) => x.initiatorType === 'xmlhttprequest');

  // get the xmlhttprequest that contains our substring/the API we are interested in
  const filmsAPI = xmlHttpRequests.filter((x) => x.name.includes(substring));
});
```

After filtering, the above should leave us with the resource we're after:

```javascript
  [
    {
      initiatorType: 'xmlhttprequest',
      ...
      name: '.../api/1.0/films',
      ...
      duration: 1391.7400000500493
    }
  ]
```

This should return an object of performance metrics for our API. The metric we're interested in is **duration** - how long this resource took to load in milliseconds. Let's grab that, and push it into an array to use later.

```javascript
cy.window().then((win) => {
  const substring = '/api/1.0/films';

  // get all xmlhttprequest resources
  const xmlHttpRequests = win.performance.getEntriesByType('resource').filter((x) => x.initiatorType === 'xmlhttprequest');

  // get the xmlhttprequest that contains our substring/the API we are interested in
  const filmsAPI = xmlHttpRequests.filter((x) => x.name.includes(substring));

  // grab the duration for that API and push it into durationArray
  durationArray.push(filmsAPI[0].duration);
});
```

Finally, write this to a JSON file so we can pass it to Plotly. The full code for this is below:

```javascript
describe('API Duration', () => {

  after(() => {
    // convert the array into JSON
    const durationJSON = JSON.stringify(durationArray);

    // declare the file path
    const path = './cypress/performance/result.json';

    // write the file
    cy.writeFile(path, durationJSON);
  })

  let durationArray = [];

  it('gets duration for /api/1.0/films', () => {
    cy.server();
    cy.route('GET', '/api/1.0/films*').as('getFilms');
    cy.visit('/films');
    cy.wait('@getFilms').then((xhr) => {
      expect(xhr.status).to.eq(200);
    });
    cy.window().then((win) => {
      const substring = '/api/1.0/films';

      // get all xmlhttprequest resources
      const xmlHttpRequests = win.performance.getEntriesByType('resource').filter((x) => x.initiatorType === 'xmlhttprequest');

      // get the xmlhttprequest that contains our substring/the API we are interested in
      const filmsAPI = xmlHttpRequests.filter((x) => x.name.includes(substring));

      // grab the duration for that API and push it into durationArray
      durationArray.push(filmsAPI[0].duration);
    });
  })
})
```

In Part 2 I'll go over how to use this data to create a bar chart using Plotly.
