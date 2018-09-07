+++
title = "Button"
weight = 1
+++

A button is a widget that enables users to trigger an action or event, such as submitting a form, opening a dialog, canceling an action, or performing a delete operation. A common convention for informing users that a button launches a dialog is to append "…" (ellipsis) to the button label, e.g., "Save as…".

In addition to the ordinary button widget, WAI-ARIA supports 2 other types of buttons:

* Toggle button: A two-state button that can be either off (not pressed) or on (pressed). To tell assistive 
technologies that a button is a toggle button, specify a value for the attribute aria-pressed. For example, a button labeled mute in an audio player could indicate that sound is muted by setting the pressed state true. Important: it is critical the label on a toggle does not change when its state changes. In this example, when the pressed state is true, the label remains "Mute" so a screen reader would say something like "Mute toggle button pressed". Alternatively, if the design were to call for the button label to change from "Mute" to "Unmute," the aria-pressed attribute would not be needed.
* Menu button: as described in the menu button pattern, a button is revealed to assistive technologies as a menu 
button if it has the property aria-haspopup set to either menu or true.

{{% note %}}
The types of actions performed by buttons are distinctly different from the function of a link (see link pattern). It is important that both the appearance and role of a widget match the function it provides. Nevertheless, elements occasionally have the visual style of a link but perform the action of a button. In such cases, giving the element role button helps assistive technology users understand the function of the element. However, a better solution is to adjust the visual design so it matches the function and ARIA role.
{{% /note %}}

## Examples
{{<demo>}}
<p>This <q>Print</q> action button uses a <code>div</code> element.</p>
<div tabindex="0"
     role="button"
     id="action">
  Print Page
</div>
<p>This <q>Mute</q> toggle button uses an <code>a</code>element.</p>
<a tabindex="0"
   role="button"
   id="toggle"
   aria-pressed="false">
  Mute
  <svg aria-hidden="true">
    <use xlink:href="/images/mute.svg#icon-sound"></use>
  </svg>
</a>
<style>
[role="button"] {
  display: inline-block;
  position: relative;
  padding: .4em .7em;
  border: 1px solid hsl(213, 71%, 49%);
  border-radius: 5px;
  box-shadow: 0 1px 2px hsl(216, 27%, 55%);
  color: #fff;
  text-shadow: 0 -1px 1px hsl(216, 27%, 25%);
  background-color: hsl(216, 82%, 51%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 53%), hsl(216, 82%, 47%));
}

[role="button"]:hover {
  border-color: hsl(213, 71%, 29%);
  background-color: hsl(216, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 33%), hsl(216, 82%, 27%));
  cursor: default;
}

[role="button"]:focus {
  outline: none;
}

[role="button"]:focus::before {
  position: absolute;
  z-index: -1;
  /* button border width - outline width - offset */
  top: calc(-1px - 3px - 3px);
  right: calc(-1px - 3px - 3px);
  bottom: calc(-1px - 3px - 3px);
  left: calc(-1px - 3px - 3px);
  border: 3px solid hsl(213, 71%, 49%);
  /* button border radius + outline width + offset */
  border-radius: calc(5px + 3px + 3px);
  content: '';
}

[role="button"]:active {
  border-color: hsl(213, 71%, 49%);
  background-color: hsl(216, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 53%), hsl(216, 82%, 47%));
  box-shadow: inset 0 3px 5px 1px hsl(216, 82%, 30%);
}

[role="button"][aria-pressed] {
  border-color: hsl(261, 71%, 49%);
  box-shadow: 0 1px 2px hsl(261, 27%, 55%);
  text-shadow: 0 -1px 1px hsl(261, 27%, 25%);
  background-color: hsl(261, 82%, 51%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 53%), hsl(261, 82%, 47%));
}

[role="button"][aria-pressed]:hover {
  border-color: hsl(261, 71%, 29%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 33%), hsl(261, 82%, 27%));
}

[role="button"][aria-pressed="true"] {
  padding-top: .5em;
  padding-bottom: .3em;
  border-color: hsl(261, 71%, 49%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 63%), hsl(261, 82%, 57%));
  box-shadow: inset 0 3px 5px 1px hsl(261, 82%, 30%);
}

[role="button"][aria-pressed="true"]:hover {
  border-color: hsl(261, 71%, 49%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 43%), hsl(261, 82%, 37%));
  box-shadow: inset 0 3px 5px 1px hsl(261, 82%, 20%);
}

[role="button"][aria-pressed]:focus::before {
  border-color: hsl(261, 71%, 49%);
}

[role="button"] svg {
  margin: .15em auto -.15em;
  height: 1em;
  width: 1em;
  pointer-events: none;
}
</style>
<script>
var ICON_MUTE_URL  = '/images/mute.svg#icon-mute';
var ICON_SOUND_URL = '/images/mute.svg#icon-sound';

function init () {
  // Create variables for the various buttons
  var actionButton = demo.getElementById('action');
  console.log(actionButton);
  var toggleButton = demo.getElementById('toggle');

  // Add event listeners to the various buttons
  actionButton.addEventListener('click', actionButtonEventHandler);
  actionButton.addEventListener('keydown', actionButtonEventHandler);

  toggleButton.addEventListener('click', toggleButtonEventHandler);
  toggleButton.addEventListener('keydown', toggleButtonEventHandler);

}

function actionButtonEventHandler (event) {
  var type = event.type;

  // Grab the keydown and click events
  if (type === 'keydown') {
    // If either enter or space is pressed, execute the funtion
    if (event.keyCode === 13 || event.keyCode === 32) {
      window.print();

      event.preventDefault();
    }
  }
  else if (type === 'click') {
    window.print();
  }
}

function toggleButtonEventHandler (event) {
  var type = event.type;

  // Grab the keydown and click events
  if (type === 'keydown') {
    // If either enter or space is pressed, execute the funtion
    if (event.keyCode === 13 || event.keyCode === 32) {
      toggleButtonState(event);

      event.preventDefault();
    }
  }
  else if (type === 'click') {
    // Only allow this event if either role is correctly set
    // or a correct element is used.
    if (event.target.getAttribute('role') === 'button' || event.target.tagName === 'button') {
      toggleButtonState(event);
    }
  }
}

function toggleButtonState (event) {
  var button = event.target;
  var currentState = button.getAttribute('aria-pressed');
  var newState = 'true';

  var icon = button.getElementsByTagName('use')[0];
  var currentIconState = icon.getAttribute('xlink:href');
  var newIconState = ICON_MUTE_URL;

  // If aria-pressed is set to true, set newState to false
  if (currentState === 'true') {
    newState = 'false';
    newIconState = ICON_SOUND_URL;
  }

  // Set the new aria-pressed state on the button
  button.setAttribute('aria-pressed', newState);
  icon.setAttribute('xlink:href', newIconState);
}
</script>
{{</demo>}}

{{% expandable label="HTML Source Code" level="3" %}}
```html
<p>This <q>Print</q> action button uses a <code>div</code> element.</p>
<div tabindex="0"
     role="button"
     id="action">
  Print Page
</div>
<p>This <q>Mute</q> toggle button uses an <code>a</code>element.</p>
<a tabindex="0"
   role="button"
   id="toggle"
   aria-pressed="false">
  Mute
  <svg aria-hidden="true">
    <use xlink:href="images/mute.svg#icon-sound"></use>
  </svg>
</a>
```
{{% /expandable %}}

{{% expandable label="CSS Source Code" level="3" %}}
```css
[role="button"] {
  display: inline-block;
  position: relative;
  padding: .4em .7em;
  border: 1px solid hsl(213, 71%, 49%);
  border-radius: 5px;
  box-shadow: 0 1px 2px hsl(216, 27%, 55%);
  color: #fff;
  text-shadow: 0 -1px 1px hsl(216, 27%, 25%);
  background-color: hsl(216, 82%, 51%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 53%), hsl(216, 82%, 47%));
}

[role="button"]:hover {
  border-color: hsl(213, 71%, 29%);
  background-color: hsl(216, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 33%), hsl(216, 82%, 27%));
  cursor: default;
}

[role="button"]:focus {
  outline: none;
}

[role="button"]:focus::before {
  position: absolute;
  z-index: -1;
  /* button border width - outline width - offset */
  top: calc(-1px - 3px - 3px);
  right: calc(-1px - 3px - 3px);
  bottom: calc(-1px - 3px - 3px);
  left: calc(-1px - 3px - 3px);
  border: 3px solid hsl(213, 71%, 49%);
  /* button border radius + outline width + offset */
  border-radius: calc(5px + 3px + 3px);
  content: '';
}

[role="button"]:active {
  border-color: hsl(213, 71%, 49%);
  background-color: hsl(216, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(216, 82%, 53%), hsl(216, 82%, 47%));
  box-shadow: inset 0 3px 5px 1px hsl(216, 82%, 30%);
}

[role="button"][aria-pressed] {
  border-color: hsl(261, 71%, 49%);
  box-shadow: 0 1px 2px hsl(261, 27%, 55%);
  text-shadow: 0 -1px 1px hsl(261, 27%, 25%);
  background-color: hsl(261, 82%, 51%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 53%), hsl(261, 82%, 47%));
}

[role="button"][aria-pressed]:hover {
  border-color: hsl(261, 71%, 29%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 33%), hsl(261, 82%, 27%));
}

[role="button"][aria-pressed="true"] {
  padding-top: .5em;
  padding-bottom: .3em;
  border-color: hsl(261, 71%, 49%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 63%), hsl(261, 82%, 57%));
  box-shadow: inset 0 3px 5px 1px hsl(261, 82%, 30%);
}

[role="button"][aria-pressed="true"]:hover {
  border-color: hsl(261, 71%, 49%);
  background-color: hsl(261, 82%, 31%);
  background-image: linear-gradient(to bottom, hsl(261, 82%, 43%), hsl(261, 82%, 37%));
  box-shadow: inset 0 3px 5px 1px hsl(261, 82%, 20%);
}

[role="button"][aria-pressed]:focus::before {
  border-color: hsl(261, 71%, 49%);
}

[role="button"] svg {
  margin: .15em auto -.15em;
  height: 1em;
  width: 1em;
  pointer-events: none;
}
```
{{% /expandable %}}

{{% expandable label="JS Source Code" level="3" %}}
```javascript
/*
*   This content is licensed according to the W3C Software License at
*   https://www.w3.org/Consortium/Legal/2015/copyright-software-and-document
*
*   File:   button.js
*
*   Desc:   JS code for Button Design Pattersn
*/

var ICON_MUTE_URL  = 'images/mute.svg#icon-mute';
var ICON_SOUND_URL = 'images/mute.svg#icon-sound';

function init () {
  // Create variables for the various buttons
  var actionButton = document.getElementById('action');
  var toggleButton = document.getElementById('toggle');

  // Add event listeners to the various buttons
  actionButton.addEventListener('click', actionButtonEventHandler);
  actionButton.addEventListener('keydown', actionButtonEventHandler);

  toggleButton.addEventListener('click', toggleButtonEventHandler);
  toggleButton.addEventListener('keydown', toggleButtonEventHandler);

}

function actionButtonEventHandler (event) {
  var type = event.type;

  // Grab the keydown and click events
  if (type === 'keydown') {
    // If either enter or space is pressed, execute the funtion
    if (event.keyCode === 13 || event.keyCode === 32) {
      window.print();

      event.preventDefault();
    }
  }
  else if (type === 'click') {
    window.print();
  }
}

function toggleButtonEventHandler (event) {
  var type = event.type;

  // Grab the keydown and click events
  if (type === 'keydown') {
    // If either enter or space is pressed, execute the funtion
    if (event.keyCode === 13 || event.keyCode === 32) {
      toggleButtonState(event);

      event.preventDefault();
    }
  }
  else if (type === 'click') {
    // Only allow this event if either role is correctly set
    // or a correct element is used.
    if (event.target.getAttribute('role') === 'button' || event.target.tagName === 'button') {
      toggleButtonState(event);
    }
  }
}

function toggleButtonState (event) {
  var button = event.target;
  var currentState = button.getAttribute('aria-pressed');
  var newState = 'true';

  var icon = button.getElementsByTagName('use')[0];
  var currentIconState = icon.getAttribute('xlink:href');
  var newIconState = ICON_MUTE_URL;

  // If aria-pressed is set to true, set newState to false
  if (currentState === 'true') {
    newState = 'false';
    newIconState = ICON_SOUND_URL;
  }

  // Set the new aria-pressed state on the button
  button.setAttribute('aria-pressed', newState);
  icon.setAttribute('xlink:href', newIconState);
}

window.onload = init;
```
{{% /expandable %}}

## Keyboard Interaction
When the button has focus:

* Space: Activates the button.
* Enter: Activates the button.
* Following button activation, focus is set depending on the type of action the button performs. For example:
  * If activating the button opens a dialog, the focus moves inside the dialog. (see dialog pattern)
  * If activating the button closes a dialog, focus typically returns to the button that opened the dialog unless the 
  function performed in the dialog context logically leads to a different element. For example, activating a cancel button in a dialog returns focus to the button that opened the dialog. However, if the dialog were confirming the action of deleting the page from which it was opened, the focus would logically move to a new context.
  * If activating the button does not dismiss the current context, then focus typically remains on the button after 
  activation, e.g., an Apply or Recalculate button.
  * If the button action indicates a context change, such as move to next step in a wizard or add another search 
 criteria, then it is often appropriate to move focus to the starting point for that action.
  * If the button is activated with a shortcut key, the focus usually remains in the context from which the shortcut key
  was activated. For example, if Alt + U were assigned to an "Up" button that moves the currently focused item in a list one position higher in the list, pressing Alt + U when the focus is in the list would not move the focus from the list.

## WAI-ARIA Roles, States, and Properties
* The button has role of button.
* The button has an accessible label. By default, the accessible name is computed from any text content inside the 
button element. However, it can also be provided with aria-labelledby or aria-label.
* If a description of the button's function is present, the button element has aria-describedby set to the ID of the element containing the description.
When the action associated with a button is unavailable, the button has aria-disabled set to true.
* If the button is a toggle button, it has an aria-pressed state. When the button is toggled on, the value of this state is true, and when toggled off, the state is false.

Sometimes just pictures of the pattern you're documenting aren't enough. Interactive patterns benefit from live demos, so that readers can test their functionality.

## Relevant WCAG Success Criteria
{{% wcag include="2.1.1" %}}