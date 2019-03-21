# General Javascript [Dev Guide]

## Code Snippets
### String Manipulation
#### Add Spaces to CamelCase String
If you have a camel case string and you wish to add a space before each capital letter, append this to the string:

```js
.split(/(?=[A-Z])/).join(' ').trim()
```
<sup>*Adds a space before each capital letter in a string, then trims the string so there are no spaces at the beggining or end.*</sup>

## Shortcuts

### If / Else (Ternary)
A standard if else statement can be written as follows:

```js
let status;

if (isActive) {
  status = 'Active';
} else {
  status = 'Disabled';
}
```

However, we can write the same code in a single line by using a ternary operator:
```js
const status = isActive ? 'Active' : 'Disabled';
```
