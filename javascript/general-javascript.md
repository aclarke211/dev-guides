# General Javascript [Dev Guide]

## Shortcuts

### If / Else
A standard if else statement can be written as follows:

```js
let status;

if (isActive) {
  status = 'Active';
} else {
  status = 'Disabled';
}
```

However, we can write the same code in a single line:
```js
const status = isActive ? 'Active' : 'Disabled';
```
