## Best Practices

### Params Object
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
browser.setSize(1280, 1024);
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

### Folder Structure
**Problem**: Whenever you are starting to write many tests, they easily become very hairy - long, complicated and w/o any proper strture. 

**Solution**: Clean and tidy folder structure, e.g.:
e2e
 |- fragments
 |- libs
 |- modals
 |- pages
 |- spec