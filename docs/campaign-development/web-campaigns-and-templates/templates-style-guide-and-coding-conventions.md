---
path: '/campaign-development/web-templates/web-templates-style-guide-and-coding-conventions'
title: 'Web Templates Style Guide and Coding Conventions'
tags: ['template']
---

This document shares best practices that Interaction Studio template developers follow for writing consistent HTML (Handlebars), CSS, and JavaScript code. Use this documentation as a guide for developing your own templates. 

## Handlebars HTML

### HTML Syntax

* Indent nested elements 4 spaces (one press of the TAB key in the template editor).

* Break long lines to improve readability, up to 120 characters maximum per line. Place HTML attributes that extend beyond 120 characters on a new continuation line, indented 4 spaces.

* Use double quotes ( ```""``` ) instead of single quotes ( ```''``` ) for HTML attributes:

```html
<a class="evg-cta evg-btn evg-btn-primary evg-btn-lg" href="/sale">
```

* The handlebars HTML content should all be contained within a single element.

* Write all code in lowercase, including HTML element names, attributes, and attribute values.

* Use HTML elements for their designed purpose. Whenever possible, use semantic elements that describe the meaning of the content. See section on "Accessibility" for more details.
  

### ID and Class Nomenclature

* Add an ID attribute to the outermost element, using a name specific to the template being built, and prefix with the `evg-` namespace (e.g. `id=“evg-hero-banner”`). Use an ID for only this outermost HTML element and do not use the same ID in more than one template.

* Use classes for all other nested elements. Prefix classes with the `evg-` namespace (e.g. `class=“evg-btn”`).

* Use hyphens to separate words in class and id names.

```html
<div id="evg-sale-banner">
    <h1 class="evg-header evg-h1">One Day Sale!</h1>
    <a class="evg-cta evg-btn evg-btn-primary" href="/sale">SHOP</a>
</div>
```

### Template HTML Classes

For suggested `evg-` prefixed class names and descriptions of their intended purpose, as well as examples of CSS styling, refer to [Template HTML Classes and CSS](/campaign-development/web-templates/web-template-html-classes-and-css).

### Attributes Order

List your HTML attributes in the following order to improve readability: 

1. `class`
2. `id`, `name`
3. `data-*`, `data-evg-*`
4. `src`, `for`, `type`, `href`, `value`
5. `title`, `alt`
6. `role`, `aria-*`
7. `style`

### Campaign Stats Tracking

If you would like to track campaign stats with Interaction Studio, use the data attributes (`data-evg-*`) available in the Handlebars HTML. These data attributes require that the Campaign Stat Tracking gear is installed and enabled for your dataset. 

Example: 

```html
<div id="evg-new-template" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}"
data-evg-user-group="{{userGroup}}">
    ...
</div>
```

See the [Campaign Stats](/campaign-development/web-templates/web-campaign-stats) article on this site to learn more.


### Accessibility

Use HTML elements for their designed purpose. Whenever possible, use semantic elements to describe the meaning of the content to developers and the browser when appropriate. For example, use heading elements (`h1`, `h2`, etc.) to identify headings, anchor elements (`a`) for navigation between pages, button elements (`button`) for actions like opening a modal, and input elements (`input`) for submit buttons (with the `type="submit"` attribute). 

Semantic element examples: `button`, `h1`, `form`, and `header`

Non-semantic element examples: `div` and `span`

If you do use non-semantic elements over semantic elements, add ARIA attributes to provide more meaning to the element. For example, if you use a `div` element instead of the `h1` element as a level 1 heading and there is more than one heading on the page, add the `role="heading"` and `aria-level="<heading level number>"` ARIA attributes to the non-semantic element (e.g. `<div role="heading" aria-level="1">Page Heading</div>`). Doing so allows assistive technologies like screen readers to identify the non-semantic element as a heading.

Provide an `alt` attribute for all image elements. In case an image fails to load, the text alternative can convey the meaning of the image in its place (e.g. `alt="Woman with a shopping cart"`). Aim for succinct, descriptive text. If the image is used purely for decoration and is not informative, set the `alt` tag to an empty string (e.g. `alt=""`) so that it can be ignored by assistive technologies. Do not leave out the `alt` attribute as some screen readers will resort to announcing the file name of the image.

For close (‘X’) buttons such as in popups, use a button element (`button`) with an `aria-label` attribute (e.g. `aria-label="Close"`) to provide an accessible name.

The [WAVE Evaluation Tool](https://chrome.google.com/webstore/detail/wave-evaluation-tool/jbbplnpkjmmeebjpijfedlgcdilocofh) Chrome extension is useful for evaluating the accessibility of your site.

For more information on accessibility, visit [Web Content Accessibility Guidelines](https://www.w3.org/TR/WCAG21/).


### Comments

For comments you wish to keep in the Handlebars-generated HTML output, use the native HTML comment format:

```html
<!-- This comment will show up as a HTML comment -->

<!-- 
    This comment will also 
    show up as a HTML comment 
-->
```

For comments you wish to remove in the resulting HTML output, use the Handlebars HTML comment format shown below. 

```handlebars
{{! Single-line comments go here. This comment will not appear in the HTML output}}

{{!
    Multi-line comments go here.
    This comment will not appear in the HTML output.
}}
```

Any Handlebars HTML comment that includes double curly braces or Handlebars expressions must follow the format below. These comments will also not appear in the resulting HTML output:

```handlebars
{{!-- This comment can contain }}, {{, or {{ ... }} --}}

{{!-- 
    Multi-line comments go here. 
    This comment will not appear in the HTML output.
    {{ handlebars expression }}
--}}
```

Visit the [Handlebars documentation](https://handlebarsjs.com/guide/#template-comments) on template comments to learn more. 

## CSS

### CSS Syntax

* Place the leading brace on the same line as the selector.

```css
/* bad */
#evg-banner .evg-cta
{
    color: red;
}

/* good */
#evg-banner .evg-cta {
    color: red;
}
```

* Add one space between the colon and value of each property. No space before the colon.

```css
/* bad */
#evg-banner .evg-cta {
    color:red;
}

/* bad */
#evg-banner .evg-cta {
    color :red;
}

/* good */
#evg-banner .evg-cta {
    color: red;
}
```

* For each declaration, indent 4 spaces (one press of the TAB key in the template editor) and include a single space after the colon. End each declaration with a semicolon ( `;` ). Add a blank line between rulesets. 

* Add one space before the leading brace.

```css
/* bad */
#evg-banner .evg-cta{
    color: red;
}

/* good */
#evg-banner .evg-cta {
    color: red;
}
```

* Use the ID selector of the outermost element for every CSS rule to scope them all to the specific template. Use classes instead of element names as selectors that follow after the ID selector.

```css
#evg-exit-intent-popup .evg-overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #000;
    opacity: .3;
}

#evg-exit-intent-popup .evg-popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
    width: 500px;
    height: 500px;
    padding: 20px;
    background-color: #fff;
    background-position: center bottom;
    background-size: cover;
    background-repeat: no-repeat;
}
```

* When using multiple selectors for a ruleset, give each selector its own line.

```css
#evg-banner .evg-header,
#evg-banner .evg-subheader {
    margin-bottom: 1rem;
    text-align: center;
}
```

* Write code in lowercase, including selectors, properties, and property values (except for strings). 

```css
/* bad */
#evg-banner .evg-message {
    color: #CED5DF;
}

/* good */
#evg-banner .evg-message {
    color: #ced5df;
}
```

* Avoid using element names in conjunction with IDs or classes. Also avoid using ancestor selectors unless necessary. 

```css
/* bad */
div#evg-banner {
    color: red;
}

/* good */
#evg-banner {
    color: red;
}

/* bad */
body div .evg-cta {
    color: red;
}

/* good */
.evg-cta {
    color: red;
}
```

* Don't add units for "0" values, unless required.

```css
/* bad */
p {
    margin: 0px 10px;
}

/* good */
p {
    margin: 0 10px;
}
```

* Don't add leading zeros in property values.

```css
/* bad */
p {
    font-size: 0.9rem;
}

/* good */
p {
    font-size: .9rem;
}
```

### Accessibility

Colors chosen for the text and background should satisfy the color contrast ratio as recommended by Web Content Accessibility Guidelines (WCAG). Aim for a color contrast ratio of 4.5 or higher for normal text and 3 for larger text (18px in bold or 24px). Visit the [WCAG documentation](https://www.w3.org/TR/WCAG21/#contrast-minimum) on minimum contrast to learn more.

Use a tool like [aremycolorsaccessible.com](https://aremycolorsaccessible.com/) to check that the color ratio meets at least the AA level.

When using a background image as the backdrop for text, set a fallback background color in case the image does not load. The fallback color should be chosen such that the text is still visible and the colors meet the color contrast ratio requirements. 

Example:

```css
#evg-hero-banner {
    background-image: url(‘image.png’);
    background-color: green;
}
```

### Declaration Order

Group properties by type, in the following order: 

1. Positioning
2. Display / Box Model
3. Color
4. Text
5. Other

Example:

```css
#evg-home-hero-banner .evg-btn {
    position: absolute;             /* Positioning */
    z-index: 10;                    /* Positioning */
    top: 0;                         /* Positioning */
    left: 0;                        /* Positioning */
    display: inline-block;          /* Display and Box Model */
    box-sizing: border-box;         /* Display and Box Model */
    width: 100px;                   /* Display and Box Model */
    margin-bottom: 10px;            /* Display and Box Model */
    padding: 10px 5px;              /* Display and Box Model */
    border-radius: 5px;             /* Display and Box Model */
    color: #fff;                    /* Color */
    background-color: #215ca0;      /* Color */
    font-size: 16px;                /* Text */
    font-family: Arial, sans-serif; /* Text */
    text-align: center;             /* Text */
    text-transform: uppercase;      /* Text */
    transition: color .15s;         /* Other */
    cursor: pointer;                /* Other */
    user-select: none;              /* Other */
}
```

### Comments

Use `/* ... */` for comments.

Single line:

```css
/* Comment goes here */
p {
    color: white;
    background-color: blue;
}
```

Multiline: 

```css
/**
 * Comment
 * goes
 * here
 */
p {
    color: red;
}
```

## Clientside JavaScript

### ES Version

Global templates are written in ES6 syntax. Use ES5 syntax if there are plans to support IE browsers.

### Syntax

* Indent nested elements by 4 spaces (one press of the TAB key in the template editor).

* Break long lines to improve readability, up to 120 characters maximum per line (including whitespace).

* Add spaces between curly brackets and their contents.
  
```javascript
// bad
const foo = {company: ‘Salesforce’}

// good
const foo = { company: ‘Salesforce’ }
```

* No spaces between square brackets and their contents.

```javascript
// bad
const foo = [ a, b, c ];

// good
const foo = [a, b, c];
```

* No spaces between parentheses and their contents.

```javascript
// bad
console.log( foo )

// good
console.log(foo)

// bad
if ( bar ) {
    console.log(bar)
}

// good
if (bar) {
    console.log(bar)
}
```

* No space between the function name and opening parenthesis of the argument list.

```javascript
// bad
function foo () {
    console.log("foo");
}

// good
function foo() {
    console.log("foo");
}
```

  * Add one space before the leading brace.

```javascript
// bad
function foo(){
    console.log("foo");
}

// good
function foo() {
    console.log("foo");
}

// bad
if (foo){
    console.log(foo);
}

// good
if (foo) {
    console.log(foo);
}
```

* Add one space before the opening parenthesis in conditional statements.
  
```javascript
// bad
if(foo) {
    console.log(foo);
}

// good
if (foo) {
    console.log(foo);
}
```

* When chaining more than two methods, separate each method by a new line and use indentation. 

```javascript
// bad
Evergage.cashDom(".evg-cta").closest(".evg-jumbotron").find(".evg-header").text("Recommended For You").css("text-align", "left");

// good
Evergage.cashDom(".evg-cta")
    .closest(".evg-jumbotron")
    .find(".evg-header")
    .text("Recommended For You")
    .css("text-align", "left");
```

### DOM Manipulation

Use ```Evergage.cashDom``` to select and manipulate elements from the DOM in a similar fashion to jQuery. 

```Evergage.cashDom(selector: <string>, context) => Cash```

Visit the [Cash](https://kenwheeler.github.io/cash/#docs) documentation to learn more.


### Deferred Rendering

Use Evergage.DisplayUtils.pageElementLoaded to defer the rendering of the template until the content zone element is loaded on page. To use this, you must have the Display Utilities gear installed and enabled for your dataset.

The observer element that monitors for the content zone element to get inserted into its DOM node is set to "body" by default. For performance optimization, this default can be overridden by adding a second selector argument, which will be used as the observer element instead.

```javascript
return Evergage.DisplayUtils.pageElementLoaded(selector).then(function(element) {
    const html = template(context);
    Evergage.cashDom(element).html(html);
    applyTheme(context);
});
```

Visit the [Template Display Utilities](/campaign-development/web-templates/web-display-utilities) documentation to learn more.

### Comments

Follow the [JSDoc](https://jsdoc.app/about-getting-started.html) standards for adding JavaScript documentation comments.

Use `//...` for inline comments. 

```javascript
const foo = function() {
    const x = 4;     
    const y = x + 2; // assign the sum of x + 2 to y
};
```

Use `/** ... */` to comment code blocks, placed it immediately before the block being documented. For multi-line comments, align the stars by indentation. 

```javascript
/** Comment goes here */
if (foo) {
    console.log(foo);
}

/**
 * Comment
 * goes
 * here
 */
function foo(context) {
    console.log(foo);
}
```

Document code blocks with one or more JSDoc tags to describe the function. 

```javascript
/**
 * @function incrementDate
 * @description Increment the date by a given number of days.
 * @param {string} date - The date, in MM/DD/YYYY format.
 * @param {number} days - The number of days to increment by.
 * @return {string} The new date, in MM/DD/YYYY format.
 */
function incrementDate(date, days) {
    // ..
}
```

See the complete list of [JSDoc tags](https://jsdoc.app/index.html#block-tags).