# Targeting Browsers [Dev Guide]
The following snippets of CSS can be used to target different browsers.

> **[NOTE]:** Targeting browsers this way is considered a *hack* and it is generally advised to find alternative solutions if possible.

## Firefox
```css
@-moz-document url-prefix() {
  // Your styling here
}
```
