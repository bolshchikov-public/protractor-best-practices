## Best Practices

### Set Screen Size
**Problem**: Whenever protractor opens the browser, we don't know the size of the opened window. This means that some visual elements that we expect to be visible - are hidden. This will lead to the failing test because your selectors will not be clickable.

**Solution**: It's quite trivial. Just set the window's width and height parameters before every test. For example,
```js
browser.driver.manage().window().setSize(1280, 1024);
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
```bash
e2e
 |- fragments
 |- libs
 |- modals
 |- pages
 |- spec
```

### Locators
**Problem**: Whenever your html is changed (removed id, different class, or another model name), your tests are going to break.

**Solution**: Use additional locators - data hooks. HTML data attributes that are set to the relevant and significant parts of the page that are used in the test.

```js
// define new locator
by.addLocator('dataHook', function (hook, optParentElement, optRootSelector) {
	var using = optParentElement || document.querySelector(optRootSelector) || document;
	return using.querySelector('[data-hook=\'' + hook + '\']');
});
```

```html
<button class="button load-more" ng-click="showMore()" data-hook="load-items">Show More</button>	
```

```js
expect(element(by.dataHook('load-items')).isDisplayed()).toBe(true);
```

### Matchers
**Problem**: When the test grows as well is logic and checks, reading the test become hard. It is not clear what the test checks. As a result different people might write duplicate tests.

**Solution**: Using jasmine matchers, we can write assertions that make much more sense and reads like almost plain English.

```js
// define assertion
this.addMatchers({
  toHaveClass: function (className) {
    var _this = this;
    return this.actual.getAttribute('class').then(function (classes) {
      helpers.createMessage(_this, 'Expected ' + classes + '{{not}}to have class ' + className);
      return classes.split(' ').indexOf(className) !== -1;
    });
  }
});
```

```js
// use
expect($('#about')).toHaveClass('collapse');
```