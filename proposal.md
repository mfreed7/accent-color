
# CSS 'accent-color' Proposal

Mason Freed</p>
July 29, 2020</p>

<br>

As discussed on [CSSWG Issue 5187](https://github.com/w3c/csswg-drafts/issues/5187), and at the [July 1, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-652700033) and [July 22, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-662570409) CSSWG meetings, there is a desire to expand the stylability of form control elements, in particular by allowing the specification of the “accent color” for various elements. A prior [study](study.md) was also performed to help guide the discussion.

This proposal is a result of those discussions.



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
"primary" accent elements. If a second &lt;color> value is provided, that color
should be used for "contrasting" accent elements. If no "contrasting" color is
provided, and the control requires a "contrasting" color for rendering, then the
UA should attempt to select an appropriate color which
offers good contrast and visibility when paired with the provided "primary"
color. The third and subsequent colors are only used on some form control elements
in some cases, for additional "accent" parts other than "primary" or
"contrasting".

Not all form elements contain “accent” parts, and not all user agents use the same
“accent” parts in exactly the same way for the same form control. However, the
intention is that if the same or similar accent parts exist on a given
form element, it should be associated with the "primary" or "contrasting" colors
in the same way across user agents. This is important to ensure good interop of
the 'accent-color' property. For that reason, there is a table of form elements
provided below, which serves as guidance on the various accent parts for each
control. While the table is not normative, it is intended to provide some
alignment and uniformity of implementation across user agents.

The <b>text content</b> of form control elements is explicitly <b>not</b> included in the set
of “accent” parts, as text content is already controlled by the <a href="https://drafts.csswg.org/css-color/#the-color-property">'color'</a> property.
In addition, the <a href="https://drafts.csswg.org/css-backgrounds-3/#background-color">'background-color'</a> property is often used to control the
rendering for some background parts of form controls - those parts are similarly
not included in the set of "accent" parts that are subject to control via the
`accent-color` property.

The default value for the 'accent-color' property is UA-defined. If the operating
system provides an “Accent Color” user setting, the UA is encouraged to respect
that setting in the initial value for ‘accent-color’. The UA may use a similar,
though not identical, color in some cases, for example to enhance contrast or
accessibility.
</pre>



# Per-Control Guidance

This section is non-normative. It describes the potential accent parts for each control, and whether those parts should be styled with the "primary", "contrasting", or "additional" `accent-color` colors. If a given form control on a given user agent has substantially different accent parts, such that the descriptions below do not apply, then care should be taken to attempt to align those parts with the described set, so that developers using `accent-color` will get predictable, interoperable results as much as possible. Note also that some of the described accent parts will not exist on all implementations - in this situation, those elements can be disregarded in the descriptions below.

## `<input type=checkbox>`

A checkbox is typically composed of a "checkmark" glyph on top of a shaded background. The glyph should be considered a "primary" accent, and the shaded background behind the glyph should utilize the "contrasting" accent color.


| Sample | CSS |
|---|---|
| ![Checkbox](proposal_files/checkbox.png) | <pre>accent-color: white blue;</pre> |



## `<input type=radio>`

A radio button is typically composed of a "dot" on top of a shaded background. The "dot" should be considered a "primary" accent, and the shaded background behind the dot should utilize the "contrasting" accent color. 


| Sample | CSS |
|---|---|
| ![Radio](proposal_files/radio.png) | <pre>accent-color: white blue;</pre> |



## `<select>`

A `<select>` control is typically displayed as a text area containing the
currently-selected `<option>` text, and an activation "widget" or arrow which is used to pop up the list of options. The `background-color` CSS property is
typically used as the background behind the currently-selected text display.
The accent part here is the activation widget/button. The arrow, icon, or
other widget should be considered the "primary" accent, while the shaded background behind the widget/button (if present) should utilize the "contrasting" accent color.

| Sample | CSS |
|--------|-----|
| ![Select](proposal_files/select.png) | <pre>accent-color: black lightblue;<br>background-color: lightgreen;</pre> |


## `<input type=text list=datalist>`

This control is typically called a "combo-box", and it includes an ordinary text input plus (sometimes) an activation widget/button which brings up a list of suggestions, very similar to a `<select>` control. The arrow, icon, or
other activation widget should be considered the "primary" accent, while the shaded background behind the widget/button should utilize the "contrasting" accent color.

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

A range input has several potential accent parts. One is the thumb, which is the part of the `<range>` that the user can drag along the track. The thumb should be considered to be a "primary" accent. If a `list=datalist` attribute is set on the `<range>`, then tickmarks are typically also drawn at the option values given in the datalist. Such tick marks should also be considered "primary" accent elements. The remaining accent element is the track that the thumb slides along, which is sometimes shaded differently on one side of the thumb vs. the other. In the case that a **single** color is used on both sides of the thumb, the track should be considered a "contrasting" accent. Sometimes, a separate color is used to shade the "filled" portion of the range, between 0 and `range.value`. In this case, a third `<color>` value can be used:

```css
accent-color: thumb-color track-color filled-track-color
```

Here, the "filled" portion of the track should be colored with `filled-track-color`.


| Sample | CSS |
|--------|-----|
| ![Range](proposal_files/range.png) | <pre>accent-color: black lightgrey blue;</pre> |



## `<progress>`

A progress bar is typically composed of a shaded track, within which a portion of the track is shaded in a different color to indicate the value of the control. The shaded background of the progress bar should be considered a "contrasting" accent, while the filled "value" portion of the progress bar should be considered "primary".

| Sample | CSS |
|--------|-----|
| ![Progress](proposal_files/progress.png) | <pre>accent-color: blue lightgrey;</pre> |



## `<input type=color>`

A color picker typically only contains a color swatch displaying the currently-selected color, and a shaded background. In most all cases, the shaded background can be controlled with the `background-color` CSS property. Since no other controls typically exist, `accent-color` will not apply. If, in some implementation, an activation widget/button exists, then that button should be considered a "primary" element, and any shaded region behind the button should be considered a "contrasting" element.


| Sample | CSS |
|--------|-----|
| ![Color](proposal_files/color.png) | N/A (`color.value` is red)|




## `<input type=color list=datalist>`

A color suggestion control, when provided, typically contains a color swatch displaying the currently-selected color, and an activation widget/button that can be used to pop up the suggestions list. The activation widget/button should be considered a "primary" accent element, while any separately-shaded background behind the widget should be considered to be a "contrasting" element.


| Sample | CSS |
|--------|-----|
| ![Color Suggestion](proposal_files/color_suggestion.png) | <pre>accent-color: black;</pre> |



## `<input type=file>`

A file picker is typically composed of a "browse" button and a separate text area for displaying the selected file(s). In most cases, the text area portion can be completely controlled with the `color` and `background-color` CSS properties. The "browse" button, on the other hand, is not typically affected by either property. Therefore, the text and/or widget on the "browse" button should be considered to be the "primary" element, while the shaded background is the "contrasting" element.

| Sample | CSS |
|--------|-----|
| ![File](proposal_files/file.png) | <pre>accent-color: white green;</pre> |



## `<textarea>`

A text area is composed of a typically-resizable rectangle for text. In most cases, the lower-right corner (for direction:ltr text) contains a "drag handle" that can be used to resize the text area. This drag handle should be considered a "primary" accent element. If the drag handle is drawn with a shaded background (not typical), then this background should be considered a "contrasting" accent element.

| Sample | CSS |
|--------|-----|
| ![Text Area](proposal_files/textarea.png) | <pre>accent-color: red;</pre> |




## `<input type=date|time|datetime-local|week|month>`

A date/time control is typically composed of a text field that displays the currently-selected date/time, and an activation widget/button which is used to bring up a pop-up "picker". The activation widget/button should be considered a "primary" accent element, while any separately-shaded background behind the widget should be considered to be a "contrasting" element. Sometimes, the date/time control will contain a "clear" button/widget, used to clear the value of the control. Similarly here, the "clear" button/widget should be considered a "primary" element, and any separately shaded background behind the widget should be considered a "contrasting" element.

| Sample | CSS |
|--------|-----|
| ![Date / Time](proposal_files/date.png) | <pre>accent-color: green;</pre> |



## `<input type=number>`

A numeric control typically has a text field displaying the value of the control, and a set of up/down buttons that can be used to change the value. The buttons often have an "arrow" glyph, which should be considered a "primary" element, while any shaded background on the buttons should be considered "contrasting" elements.


| Sample | CSS |
|--------|-----|
| ![Number](proposal_files/number.png) | <pre>accent-color: darkgrey lightgrey;</pre> |


## `<input type=search>`

A search box is typically rendered in almost the same way as a normal text box. In some cases, a "clear" widget/button is provided, to allow the user to clear the search box. In that case, the widget/button should be considered "primary", and any shaded background behind the widget should be considered "contrasting".


| Sample | CSS |
|--------|-----|
| ![Search](proposal_files/search.png) | <pre>accent-color: blue white;</pre> |


## `<input type=password>`

A password control is typically rendered in almost the same way as a normal text box. In some cases, a "clear" widget/button is provided, to allow the user to clear the search box. In other cases, a "reveal/hide" widget/button is provided to alternatively show or hide the displayed password. In both of these cases, the "clear" or "reveal" widget/button glyph should be considered "primary", while any shaded background behind the widget should be considered "contrasting".

| Sample | CSS |
|--------|-----|
| ![Password](proposal_files/password.png) | <pre>color: red;<br>accent-color: cyan;</pre> |



## `<input type=text|email|tel|url|...>`

Basic text fields (including email, telephone, etc.) are typically rendered as plain text controls, without any additional accents. In the case that accents are provided, the guidance above (for other control types) should be used to judge whether they are "primary" or "contrasting" accents.

| Sample | CSS |
|--------|-----|
| ![Text](proposal_files/text.png) | N/A |

