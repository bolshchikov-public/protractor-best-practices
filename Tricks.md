## Tricks

### Params Object
**Problem**: You have several general variables that are shared to between different tests. In case one of them changes, we don't wanna go over all tests and update the value.

**Solution**: `params` object can be used exactly for that. Set user login and password for authentication, 
```js
params = {
  username: 'Sergey'
}
```
and from the test you can use them via `brower.params.username`.


### Choose option
**Problem**: how to set a new value in select tag

**Solution**:

```js
function chooseOption(val) {
  return element(by.css('option[value="' + val + '"]')).click()
}
```

### Mouse right button click
**Problem**: Classical click is simple, but how to do right button click?

**Solution**:

```js

```