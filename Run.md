## Run and Debugging

### Intellij/WebStorm integration
**Problem**: The protractor tests are sanely difficult to debug

**Solution**: Having properly configured IDE with debug support (e.g. Intellij/Webstorm) can save many hours of pain. Just set a new run configuration according to the protractor's manual, and now you can debug your tests straight from IDE. The only small caveat: debugging doesn't work with enabled sharding capabilities.

### Sauce Labs
If case of CI, we would like to run our e2e tests on all major browsers and diffrent OS's. For that purpose, we can use Sauce Labs with credentials specified directly in protractor config file.