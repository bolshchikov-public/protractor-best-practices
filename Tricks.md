## Tricks

### Params Object
**Problem**: You have several general variables that are shared to between different tests. In case one of them changes, we don't wanna go over all tests and update the value.

**Solution**: `params` object can be used exactly for that. Set user login and password for authentication, 
```js
// protractor-conf.js

params = {
  username: 'Sergey'
}
```
and from the test you can use them via `browser.params.username`.


### Choose option
**Problem**: How to set a new value in select tag

**Solution**:

```js
function chooseOption(val) {
  return element(by.css('option[value="' + val + '"]')).click()
}
```

### Right button click
**Problem**: Classical click is simple, but how to do right button click?

**Solution**:
The `actions()` method can be used to chain the several actions and schedule them when `perform()` is called.
```js
browser.actions().mouseMove(element1).click(protractor.Button.RIGHT).perform();
browser.touchActions(). tap(element1). doubleTap(element2). perform();
```

### Run javascript code
**Problem**: Still can't find a protractor method that fits your purpose? 

**Solution**: `browser.executeAsyncScript('console.log("hello world")');`

### Test non-angular apps
**Problem**: Protractor was built with Angular in mind (additional locators, addMockModule, etc.) so it wait till Angular will finish its work (finished rendering and has no outstanding $http or $timeout calls before continuing). However, how to test your angular app that might open an iframe with non-angular app, authorization form, for instance.

**Solution**: The solution is quite simple. We can instruct Protractor not to wait for angular by `browser.ignoreSynchronization = true;`.
```js
browser.ignoreSynchronization = true;

browser.wait(picasaAuthPage.waitForEnabledApproveButton()).then(function () {
  picasaAuthPage.clickApproveButton();
  utils.switchToWindow(handles[0]);
  browser.ignoreSynchronization = false;
  done();
});
```

### Ignore IE
**Problem**: Test is fine, works in all browsers but IE due to its problems with web driver (e.g. hover)

**Solution**: Wrap your test with `runIfNotIE` method
```js
// define utility function
runIfNotIE = function (callback) {
  return browser.getCapabilities().then(function (capabilities) {
    if (capabilities.caps_.browserName !== 'internet explorer') {
      callback();
    }
  });
};
```

```js
// test
it('should check something', function () {
	runIfNotIE(function () {
		// test goes here
	});
});
```
