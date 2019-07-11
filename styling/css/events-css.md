# Events (CSS) [Dev Guide]

## Cursor Events
### Allow Clicks to Pass Through
This is useful for things like small arrows on select boxes (which are actually `::after` pseudo elements on the label for the select box).

Adding the following code will allow click events which happen on the select box to pass through the `::after` arrow, meaning the user can still click the arrow itself to open the select box.

```css
.select-box__label::after {
  pointer-events: none;
}
```

![Select Box with Custom Clickable Arrow](https://i.ibb.co/JrH5tT3/Screen-Shot-2019-07-11-at-17-17-56.png)
> The small arrow to the right of the select box will now be clickable as the click event from the select box itself is allowed to pass through.


### Pointer (for clickable items)
A good way to show to a user that an element in a page is clickable, is by changing their cusor when they hover over the element.

This does not need to be added within a `:hover` selector as the `cursor` property only comes into effect when the element is hovered anyway.

```css
.my-elem {
  cursor: pointer
}
```
