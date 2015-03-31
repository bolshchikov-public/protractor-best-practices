## Run and Debugging

### IDE Integration
**Problem**: The protractor tests are sanely difficult to debug

**Solution**: Having properly configured IDE with debug support (e.g. Intellij/Webstorm) can save many hours of pain. Just set a new run configuration according to the protractor's [manual](http://angular.github.io/protractor/#/debugging#setting-up-webstorm-for-debugging), and now you can debug your tests straight from IDE. The only small caveat: debugging doesn't work with enabled sharding capabilities.
![run-set](/images/run-set.png)
![run-debug](/images/run-debug.png)

### Element explorer
**Problem**: Something weird is going on and something doesn't work.

**Solution**: Interactive debugging with [element explorer](http://angular.github.io/protractor/#/debugging#testing-out-protractor-interactively) where can type any protractor command.

### Sauce Labs
If case of CI, we would like to run our e2e tests on all major browsers and diffrent OS's. For that purpose, we can use Sauce Labs with credentials specified directly in protractor config file.