---
title: "Site Performance With Cypress and Plotly"
date: "2020-04-09"
description: "Using Cypress and Plotly to measure and visualize site performance"
---
If you haven't heard of it, [Cypress](https://www.cypress.io/) is a fantastic E2E JavaScript testing framework. It's both straightforward to set up and use. An interesting feature of Cypress that I've been exploring recently, is that Cypress can also be used to grab site/app performance metrics.

This post will take a quick look at one way you could go about grabbing performance data during a test. Then, I'll quickly go over one way to that you could visualize that data using [Plotly](https://plotly.com/python/).


```javascript
describe('API Duration', () => {

  after(() => {
    const durationJSON = JSON.stringify(durationArray);
    const path = './cypress/performance/results.json';
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
