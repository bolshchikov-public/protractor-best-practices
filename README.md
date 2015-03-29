# Protractor: Best Practices

* Best practices
  * Set config file
    * Set browser sharding
    * Set params object
  * Set screen size
  * Page objects
  * Overrides with addMockModule()
  * Params in config 
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
Sharding property allows to share tests between different browser instances and run them in parallel. For that, we need to set two propties 
```js
shardTestFiles: false,
maxInstances: 2,
```
where `shardTestFiles` specifies whether sharding is enabled and `maxInstances` specified the number of browser instances.

#### Params Object
In case you need to set some variables that we used in many test, `params` object can be used exaclt for that. Set user login and password for authentication, 
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

### Sauce Labs
If case of CI, we would like to run our e2e tests on all major browsers and diffrent OS's. For that purpose, we can use Sauce Labs with credentials specified directly in protractor config file.