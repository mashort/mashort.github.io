---
layout: post
title: "Data Entry Forms Best Practices"
date: 2017-12-05
categories: UX
published: true
---
There's an abundance of advice from User Experience gurus on best practices to adopt when developing data entry forms for websites. Here are some best practices that make sense to me and will hopefully make for a happier user.

## Less is More
Get rid of as many fields as possible that aren't absolutely essential.

## Optional Fields
- Eliminate as many (if not all) optional fields as possible
- Clearly mark these fields as optional (as opposed to marking mandatory fields as required)
> It’s all about **Voluntary Disclosure** - by default, we humans are programmed to give away more information than we need to on forms, but marking fields as *Required* obliterates this.

## Labelling
- Top-aligned labels are best
- Left-aligned labels are worst, as this encourages lateral label-to-field eye movement and increases **Cognitive Load**
- Single-column forms are best - it's more efficient to read and work down.

## Order & Display of Fields
- Best is easiest to fill out first, to progressively harder
- Hide what is needed until it is needed

## User Orientation
Let the user know where they are / how far through they are (particularly important in multi-step forms) - consider use of progress bars.

## De-Clutter Forms
Use dynamic popups that appear (to the side) when focus is on a field or on hover - this should include information / instructions.
See:
- [jQueryUI Tooltip][jqui-tooltip]
- [Bootstrap Tooltips][bs-tooltip]
- [W3.CSS Tooltips][w3css-tooltip]

## Use in-line Validation
Do this as the user encounters each field along the way - showing a dynamic message next to each field that alerts the user to mistakes, or tells them they’re good to go. Though only validate when the user is done with it!

Ideas:
- [jQuery Validation Plugin][jq-validation-plugin]
- Consider [HTML5 Validation][html5-validation]

## Use Smart Defaults
Pre-populate fields with (sensible, best guess) defaults whenever possible - there can be a lot of things we know about the user, so why not do some of the work for them.

## Make Typing as easy as Possible
- Auto-capitalise where relevant e.g. saves user having to switch to upper case and then back again - particularly useful when the user is using a mobile device
- Auto-suggest where appropriate - take a look at the [HTML5 datalist tag][html5-datalist]

## Use Radio Buttons over Drop-Downs
Where number of options is small consider using radio options instead of drop-down lists - this reduces clicks.

## Be Clear on the Type & Format of Input Expected
Consider using placeholder text to help with this (see [HTML5 Placaholder Attribute][html5-placeholder]) - though there are mixed opinions on the use of watermarks.

## Sizing
- Use a font size of at least 14px
- Size input fields according to their expected input - data enetered into fields should all be visible


[jqui-tooltip]: https://jqueryui.com/tooltip/#forms
[bs-tooltip]: https://getbootstrap.com/docs/4.0/components/tooltips
[jq-validation-plugin]: https://jqueryvalidation.org
[html5-validation]: https://www.html5rocks.com/en/tutorials/forms/constraintvalidation
[html5-datalist]: https://www.w3schools.com/tags/tag_datalist.asp
[html5-placeholder]: https://www.w3schools.com/tags/att_input_placeholder.asp
[w3css-tooltip]: https://www.w3schools.com/w3css/w3css_tooltips.asp
