# Imports and Exports in JavaScript [Dev Guide]

## Entire JS File

### Export
We can export an entire JS file, meaning we can import the file as a module into other JS files.

```js
module.exports = () => {
  // Code to be exported
};
```

Example:
```js
module.exports = (title = 'Page Title') => {
  console.log(title);
};
```
<sup>*When imported, this export will simply log whatever 'title' is to the screen.*</sup>


### Import
To import an entire JS file (module) we just need to give the imported module a name to identify it by, and then state the location of the file we are importing.

```js
import uniqueName from './location-of-file';
```

Example:
```js
import colorLog from '../../js/color-log.js';
```

