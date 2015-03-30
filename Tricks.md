## Tricks

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