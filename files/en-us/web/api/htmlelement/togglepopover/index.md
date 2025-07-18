---
title: "HTMLElement: togglePopover() method"
short-title: togglePopover()
slug: Web/API/HTMLElement/togglePopover
page-type: web-api-instance-method
browser-compat: api.HTMLElement.togglePopover
---

{{APIRef("Popover API")}}

The **`togglePopover()`** method of the {{domxref("HTMLElement")}} interface toggles a {{domxref("Popover_API", "popover", "", "nocode")}} element (i.e., one that has a valid [`popover`](/en-US/docs/Web/HTML/Reference/Global_attributes/popover) attribute) between the hidden and showing states.

When `togglePopover()` is called on an element with the [`popover`](/en-US/docs/Web/HTML/Reference/Global_attributes/popover) attribute:

1. A {{domxref("HTMLElement/beforetoggle_event", "beforetoggle")}} event is fired.
2. The popover toggles between hidden and showing:
   1. If it was initially showing, it toggles to hidden.
   2. If it was initially hidden, it toggles to showing.
3. A {{domxref("HTMLElement/toggle_event", "toggle")}} event is fired.

## Syntax

```js-nolint
togglePopover()
togglePopover(force)
togglePopover(options)
```

### Parameters

A boolean (`force`) or an options object:

- `force` {{optional_inline}}
  - : A boolean, which causes `togglePopover()` to behave like {{domxref("HTMLElement.showPopover", "showPopover()")}} or {{domxref("HTMLElement.hidePopover", "hidePopover()")}}, except that it doesn't throw an exception if the popover is already in the target state.
    - If set to `true`, the popover is shown if it was initially hidden. If it was initially shown, nothing happens.
    - If set to `false`, the popover is hidden if it was initially shown. If it was initially hidden, nothing happens.
- `options` {{optional_inline}}
  - : An object that can contain the following properties:
    - `force` {{optional_inline}}
      - : A boolean; see the `force` description above.
    - `source` {{optional_inline}}
      - : An {{domxref("HTMLElement")}} reference; programmatically defines the invoker of the popover associated with the toggle action, that is, its control element. Establishing a relationship between a popover and its invoker using the `source` option has two useful effects:
        - The browser places the popover in a logical position in the keyboard focus navigation order when shown. This makes the popover more accessible to keyboard users (see also [Popover accessibility features](/en-US/docs/Web/API/Popover_API/Using#popover_accessibility_features)).
        - The browser creates an implicit anchor reference between the two, making it very convenient to position popovers relative to their controls using [CSS anchor positioning](/en-US/docs/Web/CSS/CSS_anchor_positioning). See [Popover anchor positioning](/en-US/docs/Web/API/Popover_API/Using#popover_anchor_positioning) for more details.

### Return value

`true` if the popup is open after the call, and `false` otherwise.

None ({{jsxref("undefined")}}) may be returned in older browser versions (see [browser compatibility](#browser_compatibility)).

## Examples

See the [Popover API examples landing page](https://mdn.github.io/dom-examples/popover-api/) to access the full collection of MDN popover examples.

### Simple auto-popup

This is a slightly modified version of the [Toggle Help UI Popover Example](https://mdn.github.io/dom-examples/popover-api/toggle-help-ui/).
The example toggles a popover on and off by pressing a particular key on the keyboard (when the example window has focus).

The HTML for the example is shown below.
This first element defines instructions on how to invoke the popup, which we need because popups are hidden by default.

```html
<p id="instructions">
  Press "h" to toggle a help screen (select example window first).
</p>
```

We then define a `<div>` element which is the popup.
The actual content doesn't matter, but note that we need the [`popover`](/en-US/docs/Web/HTML/Reference/Global_attributes/popover) attribute to make the `<div>` into a popover so that it is hidden by default (or we could set this element in the JavaScript).

```html
<div id="mypopover" popover>
  <h2>Help!</h2>

  <p>You can use the following commands to control the app</p>

  <ul>
    <li>Press <ins>C</ins> to order cheese</li>
    <li>Press <ins>T</ins> to order tofu</li>
    <li>Press <ins>B</ins> to order bacon</li>
  </ul>
</div>
```

The JavaScript for the example is shown below.
First we check whether popovers are supported, and if they aren't we hide the popover `div` so that it isn't displayed inline.

```js
const instructions = document.getElementById("instructions");
const popover = document.getElementById("mypopover");

if (!Object.hasOwn(HTMLElement.prototype, "popover")) {
  popover.innerText = "";
  instructions.innerText = "Popovers not supported";
}
```

If popovers are supported we add a listener for the `h` key to be pressed, and use that to trigger opening the popup.
We also log whether the popup was open or closed after the call, but only if a `true` or `false` was returned.

```js
if (Object.hasOwn(HTMLElement.prototype, "popover")) {
  document.addEventListener("keydown", (event) => {
    if (event.key === "h") {
      const popupOpened = popover.togglePopover();

      // Check if popover is opened or closed on supporting browsers
      if (popupOpened !== undefined) {
        instructions.innerText +=
          popupOpened === true ? `\nOpened` : `\nClosed`;
      }
    }
  });
}
```

You can test this out using the live example below.

{{EmbedLiveSample('Examples', 700, 290)}}

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [`popover`](/en-US/docs/Web/HTML/Reference/Global_attributes/popover) HTML global attribute
- [Popover API](/en-US/docs/Web/API/Popover_API)
