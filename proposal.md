
# CSS 'accent-color' Proposal

Mason Freed</p>
September 11, 2020</p>

# [[[ Note: This proposal has been [abandoned](https://github.com/w3c/csswg-drafts/issues/5480#issuecomment-709542140). ]]]

<br>

As discussed on [CSSWG Issue 5187](https://github.com/w3c/csswg-drafts/issues/5187), and at the [July 1, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-652700033), [July 22, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-662570409), [August 19, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-676546952), and [August 26, 2020](https://github.com/w3c/csswg-drafts/issues/5187#issuecomment-680996476) CSSWG meetings, there is a desire to expand the stylability of form control elements, in particular by allowing the specification of the “accent color” for various elements.

This proposal is a result of those discussions.


# Proposed Spec Text

<pre>
<b>Name</b>: ‘accent-color’
<b>Value</b>: &lt;color>+
<b>Initial</b>: 'auto'
<b>Applies to</b>: form control elements
<b>Inherited</b>: yes
<b>Percentages</b>: N/A
<b>Computed value</b>: computed color, see resolving color values
<b>Canonical order</b>: per grammar
<b>Animation type</b>: by computed value type

The ‘accent-color' CSS property sets the color of the “accent” parts or pieces
of form control elements. The first provided &lt;color> value is to be used for
"primary" accent elements. If a second &lt;color> value is provided, that color
should be used for "contrasting" accent elements.  The third and subsequent
colors are only used on some form control elements in some cases, for additional
"accent" parts other than "primary" or "contrasting".

If any color is not provided, or if 'auto' is provided for any color value, then
the UA should attempt to select an appropriate color which offers good contrast
and visibility when paired with remaining provided colors, if any. In selecting
'auto' colors, if the operating system provides an “Accent Color” or similar
user setting, the UA is encouraged to respect that setting as much as possible.
The UA may use a similar, though not identical, color in some cases, for example
to enhance contrast or accessibility.

In limited circumstances, it is permissible for user agents to render the accent
parts of some controls using different colors than those specified by
'accent-color', for example to maintain design guidelines or accessibility
constraints. In those cases, the rendered color should be influenced as much as
possible by the specified 'accent-color'. For example, the UA may wish to only
render the checkbox glyph in either white or black; in this case, the selection
of white or black should depend on the 'accent-color' value, e.g. using the
luminosity of the provided color.

Not all form elements contain “accent” parts, and not all user agents use the
same “accent” parts in exactly the same way for the same form control. However,
the intention is that if the same or similar accent parts exist on a given form
element, it should be associated with the "primary" or "contrasting" colors in
the same way across user agents. This is important to ensure good interop of the
'accent-color' property. For that reason, there is a table of form elements
provided below, which serves as guidance on the various accent parts for each
control. While the table is not normative, it is intended to provide some
alignment and uniformity of implementation across user agents.

The <b>text content</b> of form control elements is explicitly <b>not</b> included in the set
of “accent” parts, as text content is already controlled by the <a href="https://drafts.csswg.org/css-color/#the-color-property">'color'</a>
property. In addition, the <a href="https://drafts.csswg.org/css-backgrounds-3/#background-color">'background-color'</a> property is often used to control
the rendering for some background parts of form controls - those parts are
similarly not included in the set of "accent" parts that are subject to control
via the `accent-color` property.
</pre>





# Motivation and Intent

This section is non-normative.

In implementing `accent-color`, there are two competing/conflicting goals:
1. **Encourage interoperability** among browsers. It is important that developers
be able to expect similar, if not the same, behavior across browsers.
2. **Encourage** (and do not constrain) browser vendors' **innovation** of form
control elements.

The goal of interoperability pushes this solution towards a more strict
specification of exactly how to use each `accent-color` value on each form
control element. However, the goal of allowing innovation pushes this solution
**away** from strict specifications for how each form control looks or acts.
This specification attempts to provide a good middle ground, which maximizes
interoperability while minimizing constraints to innovation.

The general methodology for achieving the above compromise is to examine the set
of existing form control elements ([as of
2020](#existing-control-examples-as-of-2020)), and agree on the *basics* of the
way `accent-color` is applied to each of those existing controls. It is
*explicitly* recognized that each browser provides different implementations of
each form control, with their own look and feel. This spec does not try to
eliminate those differences. However, it does try to provide some level of
uniformity and interoperability, where commonalities exist.

This will require judgement when applying the recommendations to new or existing
form controls. The goal would be to adhere to the *spirit* of this spec as much
as possible. For example, if the guidance states that an `accent-color` value
should apply to a particular accent part, and the browser implementation of that
part uses a gradient fill rather than a single color, then there may be multiple
ways to "use" the `accent-color` value to affect the gradient rendering of that
part. The goal would be to match the guidance where it makes sense, and when
possible. That might mean replacing the gradient with a solid fill, or it might
mean changing the endpoint values of the gradient to match the `accent-color`
value *in aggregate*. Or it might mean another behavior entirely. The point is
that the guidance should be consulted and used as input in determining how to
proceed.

Further, this spec is careful to not **require** exact conformance of each form
control with the `accent-color` spec. It merely encourages browsers to follow
the guidance for form controls elements that are "close enough" to the existing
set of controls. For brand new paradigms, input surfaces, control types, etc.,
this spec does not attempt to limit innovation. If there are similar **parts**
of these new-paradigm controls, then as much as possible/reasonable, those parts
should be guided by this spec. But this should not be seen as any limitation on
innovation.

To assist in characterizing "close enough" as it relates to the accent parts of
form control elements, the [Existing Control
Examples](#existing-control-examples-as-of-2020) section of this document
includes many examples of several control types, pulled from different browsers,
operating systems, and time periods. Except where noted, each of the control
examples within each group should be considered "close enough" to the group that
the guidance for that group should apply.

# Per-Control Guidance

This section is non-normative.

It describes the potential accent parts for each control, and whether those parts should be styled with the "primary", "contrasting", or "additional" `accent-color` colors. If a given form control on a given user agent has substantially different accent parts, such that the descriptions below do not apply, then care should be taken to attempt to align those parts with the described set, so that developers using `accent-color` will get predictable, interoperable results as much as possible. Note also that some of the described accent parts will not exist on all implementations - in this situation, those elements can be disregarded in the descriptions below.

## `<input type=checkbox>`

A checkbox is typically composed of a "checkmark" glyph on top of a shaded background. The glyph should be considered a "contrasting" accent, while the shaded background behind the glyph should utilize the "primary" accent color.


| Sample | CSS |
|---|---|
| ![Checkbox](proposal_files/checkbox.png) | <pre>accent-color: blue white;</pre> |



## `<input type=radio>`

A radio button is typically composed of a "dot" on top of a shaded background. The "dot" should be considered a "contrasting" accent, while the shaded background behind the dot should utilize the "primary" accent color. 


| Sample | CSS |
|---|---|
| ![Radio](proposal_files/radio.png) | <pre>accent-color: blue white;</pre> |



## `<select>`

A `<select>` control is typically displayed as a text area containing the
currently-selected `<option>` text, and an activation "widget" or arrow which is used to pop up the list of options. The `background-color` CSS property is
typically used as the background behind the currently-selected text display.
The accent part here is the activation widget/button and the background behind it. The arrow, icon, or
other widget should be considered the "contrasting" accent, while the shaded background behind the widget/button (if present) should utilize the "primary" accent color.

| Sample | CSS |
|--------|-----|
| ![Select](proposal_files/select.png) | <pre>accent-color: lightblue black;<br>background-color: lightgreen;</pre> |


## `<input type=text list=datalist>`

This control is typically called a "combo-box", and it includes an ordinary text input plus (sometimes) an activation widget/button which brings up a list of suggestions, very similar to a `<select>` control. The arrow, icon, or
other activation widget should be considered the "contrasting" accent, while the shaded background behind the widget/button should utilize the "primary" accent color.

| Sample | CSS |
|--------|-----|
| ![Datalist](proposal_files/datalist.png) | <pre>accent-color: lightblue white;<br>background-color: darkgreen;<br>color: white;</pre> |



## `<select multiple>`

A multi-select is typically composed of a rectangular area containing several rows of text, each corresponding to a contained `<option>`. There are typically no additional widgets or accents provided; therefore, `accent-color` would typically have no affect on `<select multiple>`. However, it is possible that a UA might, for example, add checkboxes next to each option, to allow the user to select and de-select options more easily. In that case, the checkbox accents should be styled using the same rules as described for `<input type=checkbox>` above.


| Sample | CSS |
|--------|-----|
| ![Select Multiple](proposal_files/select_multiple.png) | N/A |



## `<button>`

A button does not typically have any specific accent pieces. And since `<button>` can contain arbitrary, CSS-stylable content, there will likely be no need to apply `accent-color` to the parts of `<button>`.

| Sample | CSS |
|--------|-----|
| ![Button](proposal_files/button.png) | N/A |



## `<input type=range>`

A range input has several potential accent parts. One is the thumb, which is the part of the `<range>` that the user can drag along the track. The thumb should be considered to be a "contrasting" accent. If a `list=datalist` attribute is set on the `<range>`, then tickmarks are typically also drawn at the option values given in the datalist. Such tick marks should also be considered "contrasting" accent elements. The remaining accent element is the track that the thumb slides along, which is sometimes shaded differently on one side of the thumb vs. the other. In the case that a **single** color is used on both sides of the thumb, the track should be considered a "primary" accent. Sometimes, a separate color is used to shade the "filled" portion of the range, between 0 and `range.value`. In this case, the "filled" portion of the range should be considered a "primary" accent, and a third `<color>` value can be used to shade the "other" side of the range:

```css
accent-color: filled-track-color thumb-and-ticks-color rest-of-track-color
```

Here, the "filled" portion of the track should be colored with `filled-track-color`, and the "other" side should be filled with `rest-of-track-color`.


| Sample | CSS |
|--------|-----|
| ![Range](proposal_files/range.png) | <pre>accent-color: blue black lightgrey;</pre> |



## `<progress>`

A progress bar is typically composed of a shaded track, within which a portion of the track is shaded in a different color to indicate the value of the control. The shaded background of the progress bar should be considered a "contrasting" accent, while the filled "value" portion of the progress bar should be considered "primary".

| Sample | CSS |
|--------|-----|
| ![Progress](proposal_files/progress.png) | <pre>accent-color: blue lightgrey;</pre> |



## `<input type=color>`

A color picker typically only contains a color swatch displaying the currently-selected color, and a shaded background. In most all cases, the shaded background can be controlled with the `background-color` CSS property. Since no other controls typically exist, `accent-color` will not apply. If, in some implementation, an activation widget/button exists, then that button should be treated in the same was as the activation button for `<select>`.


| Sample | CSS |
|--------|-----|
| ![Color](proposal_files/color.png) | N/A (`color.value` is `red` here)|




## `<input type=color list=datalist>`

A color suggestion control, when provided, typically contains a color swatch displaying the currently-selected color, and an activation widget/button that can be used to pop up the suggestions list. The activation widget/button should be considered a "contrasting" accent element, while any separately-shaded background behind the widget should be considered to be a "primary" element.


| Sample | CSS |
|--------|-----|
| ![Color Suggestion](proposal_files/color_suggestion.png) | <pre>accent-color: auto black;</pre> |



## `<input type=file>`

A file picker is typically composed of a "browse" button and a separate text area for displaying the selected file(s). In most cases, the text area portion can be completely controlled with the `color` and `background-color` CSS properties. The "browse" button, on the other hand, is not typically affected by either property. However, given that accent-color has no affect on a standard `<button>`, and given that there is a [proposal](https://github.com/w3c/csswg-drafts/issues/5049) to add an explicit `::file-chooser-button` pseudo-element for the button, the `'accent-color'` property should have no affect on the file picker button.

| Sample | CSS |
|--------|-----|
| ![File](proposal_files/file.png) | N/A |



## `<textarea>`

A text area is composed of a typically-resizable rectangle for text. In most cases, the lower-right corner (for direction:ltr text) contains a "drag handle" that can be used to resize the text area. This drag handle should be considered a "contrasting" accent element. If the drag handle is drawn with a shaded background (not typical), then this background should be considered a "primary" accent element.

| Sample | CSS |
|--------|-----|
| ![Text Area](proposal_files/textarea.png) | <pre>accent-color: auto red;</pre> |




## `<input type=date|time|datetime-local|week|month>`

A date/time control is typically composed of a text field that displays the currently-selected date/time, and an activation widget/button which is used to bring up a pop-up "picker". The activation widget/button should be considered a "contrasting" accent element, while any separately-shaded background behind the widget should be considered to be a "primary" element. Sometimes, the date/time control will contain a "clear" button/widget, used to clear the value of the control. Similarly here, the "clear" button/widget should be considered a "contrasting" element, and any separately shaded background behind the widget should be considered a "primary" element.

| Sample | CSS |
|--------|-----|
| ![Date / Time](proposal_files/date.png) | <pre>accent-color: auto green;</pre> |



## `<input type=number>`

A numeric control typically has a text field displaying the value of the control, and a set of up/down buttons that can be used to change the value. The buttons often have an "arrow" glyph, which should be considered a "contrasting" element, while any shaded background on the buttons should be considered "primary" elements.


| Sample | CSS |
|--------|-----|
| ![Number](proposal_files/number.png) | <pre>accent-color: lightgrey darkgrey;</pre> |


## `<input type=search>`

A search box is typically rendered in almost the same way as a normal text box. In some cases, a "clear" widget/button is provided, to allow the user to clear the search box. In that case, the widget/button should be considered "contrasting", and any shaded background behind the widget should be considered "primary".


| Sample | CSS |
|--------|-----|
| ![Search](proposal_files/search.png) | <pre>accent-color: white blue;</pre> |


## `<input type=password>`

A password control is typically rendered in almost the same way as a normal text box. In some cases, a "clear" widget/button is provided, to allow the user to clear the search box. In other cases, a "reveal/hide" widget/button is provided to alternatively show or hide the displayed password. In both of these cases, the "clear" or "reveal" widget/button glyph should be considered "contrasting", while any shaded background behind the widget should be considered "primary".

| Sample | CSS |
|--------|-----|
| ![Password](proposal_files/password.png) | <pre>color: red;<br>accent-color: auto cyan;</pre> |



## `<input type=text|email|tel|url|...>`

Basic text fields (including email, telephone, etc.) are typically rendered as plain text controls, without any additional accents. In the case that accents are provided, the guidance above (for other control types) should be used to judge whether they are "primary" or "contrasting" accents.

| Sample | CSS |
|--------|-----|
| ![Text](proposal_files/text.png) | N/A |



# Existing Control Examples (as of 2020)

This section shows visual samples of several different controls across various browsers, platforms, and eras (e.g. 2000's). The intention of presenting these examples is to provide an easy way for people to evaluate the spec text and per-controls guidance above in the context of existing controls.

Several "variations" are also shown for each control, pulled from the Mac operating system. One is with Dark Mode enabled, and the other is with the Accent Color system setting changed to a non-default color.

*Note*: The particular selections of browsers, platforms, eras, and controls were made in an attempt to show adequate diversity, while not being an exhaustive list, and while being as efficient as possible for me to collate. I have likely left out many important browsers and platforms. If there is a particular browser/platform/era/control combination that is not *represented* by a similar element in the lists below, please bring that to my attention and I can add it.


## `<input type=checkbox>`

| Browser   | Platform| Variation    | Sample |
|-----------|---------|--------------| :---:  |
| Chrome 81 | Windows |              | ![Checkbox](proposal_files/Chrome81/checkbox.png) ![Checkbox Unchecked](proposal_files/Chrome81/checkbox_empty.png) |
| Chrome 83 | Windows | (Forms Refresh) | ![Checkbox](proposal_files/Chrome83/checkbox.png) ![Checkbox Unchecked](proposal_files/Chrome83/checkbox_empty.png) |
| Safari 13 | Mac     |              | ![Checkbox](proposal_files/Safari/checkbox.png) ![Checkbox Unchecked](proposal_files/Safari/checkbox_empty.png) |
| Firefox 79| Windows |              | ![Checkbox](proposal_files/Firefox/checkbox.png) ![Checkbox Unchecked](proposal_files/Firefox/checkbox_empty.png) |
| Chrome 86 | Windows | Dark Mode    | ![Checkbox](proposal_files/Chrome86_dark/checkbox.png) ![Checkbox Unchecked](proposal_files/Chrome86_dark/checkbox_empty.png) |
| Safari 13 | Mac     | Dark Mode    | ![Checkbox](proposal_files/Safari_dark/checkbox.png) ![Checkbox Unchecked](proposal_files/Safari_dark/checkbox_empty.png) |
| Safari 13 | Mac     | Pink Accent Color | ![Checkbox](proposal_files/Safari_pink/checkbox.png) ![Checkbox Unchecked](proposal_files/Safari_pink/checkbox_empty.png) |
| Firefox 32| Windows | 2014         | ![Checkbox](proposal_files/Firefox_32/checkbox.png) ![Checkbox Unchecked](proposal_files/Firefox_32/checkbox_empty.png) |
| "Aqua"    | Mac     | 2000         | ![Checkbox](proposal_files/Aqua/checkbox.png) ![Checkbox Unchecked](proposal_files/Aqua/checkbox_empty.png) |
| Snow Leopard | Mac  | 2009         | ![Checkbox](proposal_files/Snow_leopard/checkbox.png) ![Checkbox Unchecked](proposal_files/Snow_leopard/checkbox_empty.png) |


## `<input type=radio>`

| Browser   | Platform| Variation    | Sample |
|-----------|---------|--------------| :---:  |
| Chrome 81 | Windows |              | ![Radio](proposal_files/Chrome81/radio.png) ![Radio Empty](proposal_files/Chrome81/radio_empty.png) |
| Chrome 83 | Windows | (Forms Refresh) | ![Radio](proposal_files/Chrome83/radio.png) ![Radio Empty](proposal_files/Chrome83/radio_empty.png) |
| Safari 13 | Mac     |              | ![Radio](proposal_files/Safari/radio.png) ![Radio Empty](proposal_files/Safari/radio_empty.png) |
| Firefox 79| Windows |              | ![Radio](proposal_files/Firefox/radio.png) ![Radio Empty](proposal_files/Firefox/radio_empty.png) |
| Chrome 86 | Windows | Dark Mode    | ![Radio](proposal_files/Chrome86_dark/radio.png) ![Radio Empty](proposal_files/Chrome86_dark/radio_empty.png) |
| Safari 13 | Mac     | Dark Mode    | ![Radio](proposal_files/Safari_dark/radio.png) ![Radio Empty](proposal_files/Safari_dark/radio_empty.png) |
| Safari 13 | Mac     | Pink Accent Color | ![Radio](proposal_files/Safari_pink/radio.png) ![Radio Empty](proposal_files/Safari_pink/radio_empty.png) |
| Firefox 32| Windows | 2014         | ![Radio](proposal_files/Firefox_32/radio.png) ![Radio Empty](proposal_files/Firefox_32/radio_empty.png) |
| "Aqua"    | Mac     | 2000         | ![Radio](proposal_files/Aqua/radio.png) ![Radio Empty](proposal_files/Aqua/radio_empty.png) |
| Snow Leopard | Mac  | 2009         | ![Radio](proposal_files/Snow_leopard/radio.png) ![Radio Empty](proposal_files/Snow_leopard/radio_empty.png) |


## `<select>`

| Browser   | Platform| Variation    | Sample |
|-----------|---------|--------------| :---:  |
| Chrome 81 | Windows |              | ![Select](proposal_files/Chrome81/select.png) |
| Chrome 83 | Windows | (Forms Refresh) | ![Select](proposal_files/Chrome83/select.png) |
| Safari 13 | Mac     |              | ![Select](proposal_files/Safari/select.png) |
| Firefox 79| Windows |              | ![Select](proposal_files/Firefox/select.png) |
| Chrome 86 | Windows | Dark Mode    | ![Select](proposal_files/Chrome86_dark/select.png) |
| Safari 13 | Mac     | Dark Mode    | ![Select](proposal_files/Safari_dark/select.png) |
| Safari 13 | Mac     | Pink Accent Color | ![Select](proposal_files/Safari_pink/select.png) |
| Firefox 32| Windows | 2014         | ![Select](proposal_files/Firefox_32/select.png) |
| "Aqua"    | Mac     | 2000         | ![Select](proposal_files/Aqua/select.png) |
| Snow Leopard | Mac  | 2009         | ![Select](proposal_files/Snow_leopard/select.png) |


## `<input type=date>`

| Browser   | Platform| Variation    | Sample |
|-----------|---------|--------------| :---:  |
| Chrome 81 | Windows |              | ![Date Picker](proposal_files/Chrome81/date.png) |
| Chrome 83 | Windows | (Forms Refresh) | ![Date Picker](proposal_files/Chrome83/date.png) |
| Safari 13 | Mac     |              | ![Date Picker](proposal_files/Safari/date.png) |
| Firefox 79| Windows |              | ![Date Picker](proposal_files/Firefox/date.png) |
| Chrome 86 | Windows | Dark Mode    | ![Date Picker](proposal_files/Chrome86_dark/date.png) |
| Safari 13 | Mac     | Dark Mode    | ![Date Picker](proposal_files/Safari_dark/date.png) |
| Safari 13 | Mac     | Pink Accent Color | ![Date Picker](proposal_files/Safari_pink/date.png) |
| Firefox 32| Windows | 2014         | ![Date Picker](proposal_files/Firefox_32/date.png) |
| Snow Leopard | Mac  | 2009         | ![Date Picker](proposal_files/Snow_leopard/date.png) |


## `<button>`

| Browser   | Platform| Variation    | Sample |
|-----------|---------|--------------| :---:  |
| Chrome 81 | Windows |              | ![Button](proposal_files/Chrome81/button.png) |
| Chrome 83 | Windows | (Forms Refresh) | ![Button](proposal_files/Chrome83/button.png) |
| Safari 13 | Mac     |              | ![Button](proposal_files/Safari/button.png) |
| Firefox 79| Windows |              | ![Button](proposal_files/Firefox/button.png) |
| Chrome 86 | Windows | Dark Mode    | ![Button](proposal_files/Chrome86_dark/button.png) |
| Safari 13 | Mac     | Dark Mode    | ![Button](proposal_files/Safari_dark/button.png) |
| Safari 13 | Mac     | Pink Accent Color | ![Button](proposal_files/Safari_pink/button.png) |
| Firefox 32| Windows | 2014         | ![Button](proposal_files/Firefox_32/button.png) |
| "Aqua"    | Mac     | 2000         | ![Button](proposal_files/Aqua/button.png) |
| Snow Leopard | Mac  | 2009         | ![Button](proposal_files/Snow_leopard/button.png) |


