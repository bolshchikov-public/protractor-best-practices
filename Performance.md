## Performance

### Sharding
**Problem**: You have too many tests and now it takes too much time to run them.

**Solution**: Sharding property allows to share tests between different browser instances and run them in parallel. For that, we need to set two propties 
```js
shardTestFiles: false,
maxInstances: 2,
```
where `shardTestFiles` specifies whether sharding is enabled and `maxInstances` specified the number of browser instances.

### Diable animations
**Problem**: See above

**Solution**: Disable animation in `onPrepare` callback

```js
onPrepare: function () {
  var disableNgAnimate = function () {
    angular.module('disableNgAnimate', []).run(function ($animate) {
      $animate.enabled(false);
    });
  };

  browser.addMockModule('disableNgAnimate', disableNgAnimate);

  var disableCssAnimate = function () {
    angular.module('disableCssAnimate', []).run(function () {
      var style = document.createElement('style');
      style.type = 'text/css';
      style.innerHTML = '* {' +
        '-webkit-transition: none !important;' +
        '-moz-transition: none !important;' +
        '-o-transition: none !important;' +
        '-ms-transition: none !important;' +
        'transition: none !important;' +
        '}';
      document.getElementsByTagName('head')[0].appendChild(style);
    });
  };

  browser.addMockModule('disableCssAnimate', disableCssAnimate);
}
```

### Checks amount
**Problem**: See above

**Solution**: The right balance between amount of `expect`s in `it` clause. As a rule of thumb: 4-5.