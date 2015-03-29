# Protractor: Best Practices

* Best practices
  * Set config file
    * Set browser sharding
    * Set params object
  * Set screen size
  * Page objects
  * Overrides with addMockModule()
  * Turned off animation be default
* Tricks
  * Choose option
  * Mouse hover
  * Right mouse button click
  * ifNotIE, ifNotFirefox function
  * browser.executeAsyncScript method
  * browser.ignoreSynchronization method
  * [data-hooks for HTML](https://github.com/wix/wix-protractor-helpers/blob/master/src/locators.js)
  * [Macthers](https://github.com/wix/wix-protractor-helpers/blob/master/src/matchers.js)
* Run & Debugging
  * [Node run configuration](https://github.com/angular/protractor/blob/master/docs/debugging.md#setting-up-webstorm-for-debugging) in WebStorm/Intellij
  * [Element explorer](https://github.com/angular/protractor/blob/master/docs/debugging.md#testing-out-protractor-interactively)
  * [Elementor](https://github.com/andresdominguez/elementor)
  * Sauce Labs & tunnel, and screenshots
  
---

### Set Config File
The configuration file is the heart of running protractor. Thus, proper settings might make our life much easier. 

#### Sharding
**Problem**: You have too many tests and now it takes too much time to run them.

**Solution**: Sharding property allows to share tests between different browser instances and run them in parallel. For that, we need to set two propties 
```js
shardTestFiles: false,
maxInstances: 2,
```
where `shardTestFiles` specifies whether sharding is enabled and `maxInstances` specified the number of browser instances.

#### Params Object
**Problem**: You have several general variables that are shared to between different tests. In case one of them changes, we don't wanna go over all tests and update the value.

**Solution**: `params` object can be used exactly for that. Set user login and password for authentication, 
```js
params = {
  username: 'Sergey'
}
```
and from the test you can use them via `brower.params.username`.

### Set Screen Size
**Problem**: Whenever protractor opens the browser, we don't know the size of the opened window. This means that some visual elements that we expect to be visible - are hidden. This will lead to the failing test because your selectors will not be clickable.

**Solution**: It's quite trivial. Just the the window's width and height parameters before every test. For example,
```js
browser.setSize(1024, 768);
```

### Page Objects
**Problem**: When your test have many locators that are repeated in different tests, a tiny change might cause a painfull process of fixing many tests. 

**Solution**: [Page Objects](http://martinfowler.com/bliki/PageObject.html) and Page Fragements introduce additional level of abstraction which describes the structure of your page. Object contains semantically siginificant structural parts parts like lists, as well as action that can be done on the page, e.g. filling the form, submitting, etc.
```js
// filters fragment
function FiltersFragment() {
  this.container = $('#filters');
  this.label = this.container.$('header');
  this.items = this.container.$$('.item');

  this.chooseItem = function (value) {
    // ... logic goes here
  }
}
``` 

```js
// how to use
var Filters = require('../pages/filters');
describe('Filters Page', function () {
  
  var filters;
  beforeEach(function () {
    browser.get('/');
    filters = new Filters();
  });

  it('should have 8 filters', function () {
    expect(filters.items.count()).toEqual(8);
  });
  
});
```

### Sauce Labs
If case of CI, we would like to run our e2e tests on all major browsers and diffrent OS's. For that purpose, we can use Sauce Labs with credentials specified directly in protractor config file.