
# CSS 'accent-color' Proposal

Mason Freed</p>
July 28, 2020</p>

<br>

As discussed on [CSSWG Issue 5187](https://github.com/w3c/csswg-drafts/issues/5187), and at the [July 1, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-652700033) and [July 22, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-662570409) CSSWG meetings, there is a desire to expand the stylability of form control elements, in particular by allowing the specification of the “accent color” for various elements. A prior [study](study.md) was also performed to help guide the discussion.

This proposal is the result of those discussions.



# Proposed Spec Text

<pre>
<b>Name</b>: ‘accent-color’
<b>Value</b>: &lt;color>+
<b>Initial</b>: UA-chosen value
<b>Applies to</b>: form control elements
<b>Inherited</b>: yes
<b>Percentages</b>: N/A
<b>Computed value</b>: computed color, see resolving color values
<b>Canonical order</b>: per grammar
<b>Animation type</b>: by computed value type

The ‘accent-color' CSS property sets the color of the “accent” parts or pieces of
form control elements. The first provided &lt;color> value is to be used for
"foreground" accent elements. If a second &lt;color> value is provided, that color
should be used for "background" accent elements. If no "background" color is
provided, the UA should attempt to select an appropriate background color which
offers good contrast and visibility when paired with the provided foreground color.

Not all form elements contain “accent” parts, and not all user agents use the same
“accent” parts in exactly the same way for the same form control. However, the
intention is that if the same or similar accent parts exist on a given
form element, it should be associated with the "foreground" or "background" colors
in the same way across user agents. This is important to ensure good interop of
the 'accent-color' property. For that reason, there is a table of form elements
provided below, which serves as guidance on the various accent parts for each
control. While the table is not normative, it is intended to provide some
alignment across user agents.

The <b>text content</b> of form control elements are explicitly <b>not</b> included in the set
of “accent” parts, as text content is already controlled by the <a href="https://drafts.csswg.org/css-color/#the-color-property">color</a> property.

The default value for the 'accent-color' property is UA-defined. If the operating
system provides an “Accent Color” user setting, the UA is encouraged to respect
that setting in the initial value for ‘accent-color’. The UA may use a similar,
though not identical, color in some cases, for example to enhance contrast or
accessibility.
</pre>



# Per-Control Guidance

This section is non-normative. It describes the potential accent parts for each
control, and whether those parts should be styled with the "foreground" and/or
"background" `accent-color` colors. If a given form control on a given user
agent has substantially different accent parts, care should be taken to attempt
to align those parts with the existing set, so that developers using `accent-color`
will get predictable, interoperable results, as much as possible.

## `<input type=checkbox>`

A checkbox is typically composed of a "checkmark" glyph on top of a shaded background. The glyph should be considered a "foreground" accent, and the shaded background behind the glyph should utilize the "background" accent color.


| Sample | CSS |
|---|---|
| ![Checkbox](proposal_files/checkbox.png) | <pre>accent-color: white blue;</pre> |



## `<input type=radio>`

A radio button is typically composed of a "dot" on top of a shaded background. The "dot" should be considered a "foreground" accent, and the shaded background behind the dot should utilize the "background" accent color. 


| Sample | CSS |
|---|---|
| ![Radio](proposal_files/radio.png) | <pre>accent-color: white blue;</pre> |



## `<select>`

A `<select>` control is typically displayed as a text area containing the
currently-selected `<option>` text, and an activation "widget" or arrow which is used to pop up the list of options. The `background-color` CSS property is
typically used as the background behind the currently-selected text display.
The accent part here is the activation widget/button. The arrow, icon, or
other widget should be considered the "foreground" accent, while the shaded background behind the widget/button should utilize the "background" accent color.

| Sample | CSS |
|--------|-----|
| ![Select](proposal_files/select.png) | <pre>accent-color: black lightblue;<br>background-color: lightgreen;</pre> |


## `<input type=text list=datalist>`

This control is typically called a "combo-box", and it includes an ordinary text input plus (sometimes) an activation widget/button which brings up a list of suggestions, very similar to a `<select>` control. The arrow, icon, or
other activation widget should be considered the "foreground" accent, while the shaded background behind the widget/button should utilize the "background" accent color.

| Sample | CSS |
|--------|-----|
| ![Datalist](proposal_files/datalist.png) | <pre>accent-color: white blue;<br>background-color: darkgreen;<br>color: white;</pre> |



## `<select multiple>`

A multi-select is typically composed of a rectangular area containing several rows of text, each corresponding to a contained `<option>`. There are typically no additional widgets or accents provided; therefore, `accent-color` would typically have no affect on `<select multiple>`. However, it is possible that a UA might, for example, add checkboxes next to each option, to allow the user to select and de-select options more easily. In that case, the checkbox accents should be styled using the same rules as described for checkbox above.


| Sample | CSS |
|--------|-----|
| ![Select Multiple](proposal_files/select_multiple.png) | N/A |



## `<button>`

A button does not typically have any specific accent pieces. And since `<button>` can contain arbitrary, CSS-stylable content, there will likely be no need to apply `accent-color` to the parts of `<button>`.

| Sample | CSS |
|--------|-----|
| ![Button](proposal_files/button.png) | N/A |



## `<input type=range>`

A range input has several potential accent parts. The first is the thumb, which is the part of the `<range>` that the user can drag along the track. The thumb should be considered to be a "foreground" accent. The second is the track that the thumb slides along, which is sometimes shaded differently on one side of the thumb vs. the other. In the case that a single color is used on both sides, the track should be considered a "background" accent. In different colors are used on either side of the thumb, see below. If a `list=datalist` attribute is provided, then tickmarks are typically drawn at the option values given in the datalist - those tick marks should be considered "foreground" accent elements.

Sometimes, a separate color is used to shade the portion of the range between 0 and `range.value`. In this case, this "filled" portion of the track can be controlled with a third `<color>` value for the `accent-color` property:

```css
accent-color: thumb-color track-color filled-track-color
```


| Sample | CSS |
|--------|-----|
| ![Range](proposal_files/range.png) | <pre>accent-color: white darkgrey darkgrey;</pre> |



<hr>
Work in progress below...


## `<XXXXXXXXXX>`

A XXXX is typically composed of 

| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |



## `<XXXXXXXXXX>`

A XXXX is typically composed of 


| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |


## `<XXXXXXXXXX>`

A XXXX is typically composed of 

| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |



## `<XXXXXXXXXX>`

A XXXX is typically composed of 


| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |


## `<XXXXXXXXXX>`

A XXXX is typically composed of 

| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |



## `<XXXXXXXXXX>`

A XXXX is typically composed of 


| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |


## `<XXXXXXXXXX>`

A XXXX is typically composed of 

| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |



## `<XXXXXXXXXX>`

A XXXX is typically composed of 


| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |


## `<XXXXXXXXXX>`

A XXXX is typically composed of 

| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |



## `<XXXXXXXXXX>`

A XXXX is typically composed of 


| Sample | CSS |
|--------|-----|
| ![XXXXXXX](proposal_files/XXXXXXX.png) | <pre>accent-color: XXXXXXX XXXXXXX</pre> |






