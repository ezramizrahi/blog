---
title: "Cypress - E2E Testing And Race Conditions"
date: "2019-10-16"
---

For the past 7 months I've been using Cypress as my E2E testing tool. While
there are some things I'd like to see implemented (e.g. support for native browser events, support for Firefox), I have few complaints. Cypress is really enjoyable to use.

In the early stages of writing tests using Cypress, I found many of my tests failing - even though
during manual testing nothing was wrong. In a lot of these cases, race conditions (i.e. the application acting too slow, and Cypress acting too fast) was the cause for the failures. Here are some useful methods I've learned to guard against failures due to race conditions in Cypress.

### cy.wait(...)
Let's say you're using `cy.get()` to check if a list of articles has appeared on your page. But for whatever reason, the data didn't load so quickly this time, and your `cy.get()` yields nothing, and the test fails. Using `cy.wait('@someApi')` (which automatically tests for a 200 server response) will help guard against checking for data that may not exist yet.

If you want to be more certain that data you're checking for is actually available, you can use a test like

```
cy.wait('@someApi').then((xhr) => {
  expect(xhr.response.body.data).to.not.be.empty;
});
```

### Set up guards
Again, let's say you're using `.click()` on a button on the page. Again, for whatever reason, maybe the button has not yet been rendered and Cypress is already trying to `cy.get()` it and `.click()` it. If the button isn't there and the command timeout has been reached, the test will fail. Before you try to `.click()` on a button for example, test that it exists in the DOM with something like

```
cy.get('button').should('exist');
```
or
```
cy.get('button').should('be.visible');
```

There's lots of ways you can go about setting up guards, those are just two simple ideas.
