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

Another way to do this (if you only have 1 function in the .js file) is with export default:

```js
export default function functionName() {
  // Code to be exported
}
```

Export an object:
```js
export default {
  // Code to be exported
}
```


### Import
To import an entire JS file (module) we just need to give the imported module a name to identify it by, and then state the location of the file we are importing.

```js
import uniqueName from './location-of-file';
```

Example:
```js
import colorLog from '../../js/color-log.js';
```


<br><br>
## Individual Functions

### Export
We do not always have to export an entire file. We can instead choose certain function from with a JS file which can be imported and accessed in other files.

```js
export function functionName() {
  // Code here
}
```


### Import
To import individual files, we need to state the name of the function <sub>(inside curly brackets)</sub>, followed by the location of the file which includes the export function.

We are essentailly de-structuring the functions from within the external javascript file <sub>(i.e. saying create me a function called myFunction based on the function also called myFunction in the external file)</sub>.

```js
import { myFunction } from '../location-of-file';
```
<sup>*Note how the name of the function being imported is the same as the name of the export function.*</sup>
