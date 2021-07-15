---
path: '/campaign-development/web-templates/web-template-html-classes-and-css'
title: 'Web Template CSS'
tags: ['template']
---

Interaction Studio web templates support any valid [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) styling for the target browsers of your website.

## HTML Classes and CSS Styles

This article presents suggested `evg-` prefixed HTML class names and descriptions of their intended purpose, as well as examples of CSS styling. If more classes than those listed here are needed, refer to [Bootstrap classes](https://getbootstrap.com/docs/4.5/getting-started/introduction/) for ideas. It is advised to prefix all classes with `evg-` so they do not conflict with styling already on your website.


### Jumbotron Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-jumbotron</td>
    <td>The parent container class of a jumbotron-style hero banner.</td>
    <td>
<pre lang="css">
.evg-jumbotron {
    position: relative;
    z-index: 2;
    display: -webkit-flex;
    display: flex;
    -webkit-flex-flow: column nowrap;
    flex-flow: column nowrap;
    -webkit-justify-content: center;
    justify-content: center;
    -webkit-align-items: center;
    align-items: center;
    width: 100%;
    height: 500px;
    padding: 10px;
    background-size: cover;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-cta</td>
    <td>Applies styling for call-to-action buttons.</td>
    <td>-</td>
  </tr>
</table>


### Product Recommendation Classes

<table>
  <tr">
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-product-recommendations</td>
    <td>The parent container class of a product recommendations block. Applies styling for the layout of all product recommendations.</td>
    <td>
<pre lang="css">
.evg-product-recommendations {
    position: relative;
    z-index: 2;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
    -ms-flex-flow: row nowrap;
    flex-flow: row nowrap;
    -webkit-box-pack: space-between;
    -ms-flex-pack: space-between;
    justify-content: space-between;
    -webkit-box-align: stretch;
    -ms-flex-align: stretch;
    align-items: stretch;
    width: auto;
    height: 100%;
    padding: 0;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-recommendation</td>
    <td>The container class of a single product recommendation. Applies styling for the layout of the content within each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-recommendation {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-flow: column nowrap;
    flex-flow: column nowrap;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: end;
    -ms-flex-align: end;
    align-items: flex-end;
    width: auto;
    margin: 0;
    padding: 0 .5rem;
    text-align: center;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-image</td>
    <td>Applies styling for the product image for each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-image {
    width: 80%;
    height: auto;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-details</td>
    <td>Applies styling for the body of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-details {
    width: 100%;
    height: 100%;
    min-height: 9rem;
    padding: .5rem 0;
  }
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-name</td>
    <td>Applies styling for the product name of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-name {
    display: block;
    max-width: 90%;
    min-height: 2.5rem;
    margin: auto auto .25rem;
    color: #393939;
    font-size: 1rem;
    font-weight: 600;
    line-height: 1.25rem;
    text-decoration: none;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-description</td>
    <td>Applies styling for the product description of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-description {
    overflow: hidden;
    width: 95%;
    max-height: 2rem;
    margin: 0 auto;
    font-size: .85rem;
    line-height: 1rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-price</td>
    <td>Applies styling for the product price of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-price {
    margin-bottom: .5rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-product-rating</td>
    <td>Applies styling for the product rating of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-product-rating {
    width: 5rem;
    margin: 0 auto .5rem auto;
    text-align: left;
    text-decoration: none;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-star-empty</td>
    <td>Applies styling for the empty rating stars of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-star-empty {
    color: #dee3e8;
    background-color: #ffffff;
    text-align: left;
    text-decoration: none;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-star-fill</td>
    <td>Applies styling for filled rating stars of each product recommendation.</td>
    <td>
<pre lang="css">
.evg-star-fill {
    position: relative;
    overflow: hidden;
    margin-top: -1.4em;
    color: #ce763c;
    background-color: transparent;
}
</pre>
    </td>
  </tr>
</table>


### Content Recommendation Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-content-recommendations</td>
    <td>The parent container class of a content recommendations block. Applies styling for the layout of all content recommendations.</td>
    <td>
<pre lang="css">
.evg-content-recommendations {
    position: relative;
    z-index: 2;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
    -ms-flex-flow: row wrap;
    flex-flow: row wrap;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: start;
    -ms-flex-align: start;
    align-items: flex-start;
    width: auto;
    max-width: 960px;
    margin: 0 auto;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-content-recommendation</td>
    <td>The container class of a single content recommendation.</td>
    <td>
<pre lang="css">
.evg-content-recommendation {
    margin: 1.5rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-content-image</td>
    <td>Applies styling for the content image of each content recommendation.</td>
    <td>
<pre lang="css">
.evg-content-image {
    display: block;
    width: 100%;
    height: auto;
    margin: 0;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-content-details</td>
    <td>Applies styling for the body of each content recommendation.</td>
    <td>
<pre lang="css">
.evg-content-details {
    overflow: hidden;
    width: 100%;
    padding: 1rem;
    background-color: #fff;
    text-align: left;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-content-title</td>
    <td>Applies styling for the content title of each content recommendation.</td>
    <td>
<pre lang="css">
.evg-content-title {
    overflow: hidden;
    height: 2.5rem;
    margin: 0 auto .75rem auto;
    font-size: 1.5rem;
    font-weight: 600;
    line-height: 2.5rem;
    text-decoration: none;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-content-description</td>
    <td>Applies styling for the content description of each content recommendation.</td>
    <td>
<pre lang="css">
.evg-content-description {
    overflow: hidden;
    height: 8rem;
    font-size: 1.25rem;
    line-height: 2rem;
}
</pre>
    </td>
  </tr>
</table>


### Form Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-form</td>
    <td>The parent container class of a form.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-error-msg</td>
    <td>Applies styling for error message texts.</td>
    <td>
<pre lang="css">
.evg-error-msg {
    color: #ff0000;
    font-size: 1rem;
}
</pre>
</td>
  </tr>
  <tr>
    <td>.evg-opt-out-msg</td>
    <td>Applies styling for an opt-out message.</td>
    <td>
<pre lang="css">
.evg-opt-out-msg {
    font-size: 1rem;
    text-decoration: underline;
    cursor: pointer;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-email</td>
    <td>Applies styling for an email input field.</td>
    <td>
<pre lang="css">
.evg-email {
    color: #ff0000;
    font-size: 1rem;
}
</pre>
</td>
  </tr>
</table>


### Modal Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-modal</td>
    <td>The parent container class of a modal.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-overlay</td>
    <td>Applies a semi-transparent layer over the page, behind a popup or a modal.</td>
    <td>
<pre lang="css">
.evg-overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: .3;
    width: 100%;
    height: 100%;
    background-color: #000;
}
</pre>
    </td>
  </tr>
</table>


### Popup Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-popup</td>
    <td>The parent container class of a popup.</td>
    <td>
<pre lang="css">
.evg-popup {
    position: fixed;
    top: 50%;
    left: 50%;
    width: 500px;
    height: 500px;
    padding: 20px;
    background-color: #fff;
    background-position: center bottom;
    background-size: cover;
    background-repeat: no-repeat;
    transform: translateX(-50%) translateY(-50%);
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-cta</td>
    <td>Applies styling for call-to-action buttons.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-overlay</td>
    <td>Applies a semi-transparent layer over the page, behind a popup or a modal.</td>
    <td>
<pre lang="css">
.evg-overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    opacity: .3;
    width: 100%;
    height: 100%;
    background-color: #000;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-main-panel</td>
    <td>The container class of the main panel of a popup. Applies styling for the layout of the content within the panel.</td>
    <td>
<pre lang="css">
.evg-main-panel {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;    
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    width: 100%;
    height: 100%;
    text-align: center;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-confirm-panel</td>
    <td>The container class of the secondary confirmation panel of a popup. Applies styling for the layout of the content within the panel.</td>
    <td>
<pre lang="css">
.evg-confirm-panel {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;    
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    width: 100%;
    height: 100%;
    text-align: center;
}
</pre>
    </td>
  </tr>
</table>


### Carousel Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-carousel</td>
    <td>The parent container class of a carousel.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-carousel-slide</td>
    <td>The container class of a single carousel slide.</td>
    <td>-</td>
  </tr>
</table>


### Button Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-btn</td>
    <td>Applies basic styling to the button, including spacing and sizing. Use class on button, anchor, or input elements.</td>
    <td>
<pre lang="css">
.evg-btn {
    display: inline-block;
    vertical-align: middle;
    padding: .5rem 1rem;
    border: 1px solid transparent;
    border-radius: 3rem;
    color: #215ca0;
    background-color: transparent;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    text-align: center;
    text-decoration: none;
    cursor: pointer;
    transition: 
        color .15s ease-in-out, 
        background-color .15s ease-in-out, 
        border-color .15s ease-in-out, 
        box-shadow .15s ease-in-out;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-primary</td>
    <td>Applies a solid primary color background to the button, adding more visual weight.</td>
    <td>
<pre lang="css">
.evg-btn-primary {
    color: #ffffff;
    border-color: #215ca0;
    background-color: #215ca0;
    transition: background-color .15s, color .15s;
}
</pre>
Hover state:
<pre lang="css">
.evg-btn-primary:hover {
    color: #ffffff;
    border-color: #1c4e88;
    background-color: #1c4e88;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-secondary</td>
    <td>Applies a solid secondary color background to the button, adding more visual weight.</td>
    <td>
<pre lang="css">
.evg-btn-secondary {
    color: #ffffff;
    border-color: #6c757d;
    background-color: #6c757d;
    transition: background-color .15s, color .15s;
}
</pre>
Hover state:
<pre lang="css">
.evg-btn-secondary:hover {
    color: #ffffff;
    border-color: #59595c;
    background-color: #59595c;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-outline-primary</td>
    <td>Removes the background color on any button (transparent) and colors the text and border using the primary color.</td>
    <td>
<pre lang="css">
.evg-btn-outline-primary {
    color: #215ca0;
    border-color: #215ca0;
    transition: background-color .15s, color .15s;
}
</pre>
Hover state:
<pre lang="css">
.evg-btn-outline-primary:hover {
    color: #ffffff;
    border-color: #215ca0;
    background-color: #215ca0;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-outline-secondary</td>
    <td>Removes the background color on any button (transparent) and colors the text and border using the secondary color.</td>
    <td>
<pre lang="css">
.evg-btn-outline-secondary {
    color: #6c757d;
    border-color: #6c757d;
    transition: background-color .15s, color .15s;
}
</pre>
Hover state:
<pre lang="css">
.evg-btn-outline-secondary:hover {
    color: #ffffff;
    border-color: #6c757d;
    background-color: #6c757d;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-sm</td>
    <td>Decreases the default button size.</td>
    <td>
<pre lang="css">
.evg-btn-sm {
    padding: .25rem .75rem;
    font-size: .875rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-lg</td>
    <td>Increases the default button size.</td>
    <td>
<pre lang="css">
.evg-btn-lg {
    padding: .5rem 2rem;
    font-size: 1.25rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-btn-dismissal</td>
    <td>Styles the button into a "X" symbol.
    Whenever possible, use this class on a `button` element with an `aria-label="Close"`.</td>
  <td>
<pre lang="css">
.evg-btn-dismissal {
    position: absolute;
    top: 0;
    right: 0;
    padding: 0 .5rem;
    border: 1px solid transparent;
    background-color: transparent;
    font-size: 2rem;
    font-weight: 700;
    line-height: 1;
    cursor: pointer;
}
</pre>
    </td>
  </tr>
</table>


### Heading Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-header</td>
    <td>Applies unique styling for headers.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-subheader</td>
    <td>Applies unique styling for subheaders.</td>
    <td>-</td>
  </tr>
  <tr>
    <td>.evg-h1</td>
    <td>Applies styling for a level 1 heading.</td>
    <td>
<pre lang="css">
.evg-h1 {
    font-size: 2.5rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-h2</td>
    <td>Applies styling for a level 2 heading.</td>
    <td>
<pre lang="css">
.evg-h2 {
    font-size: 2rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-h3</td>
    <td>Applies styling for a level 3 heading.</td>
    <td>
<pre lang="css">
.evg-h3 {
    font-size: 1.75rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-h4</td>
    <td>Applies styling for a level 4 heading.</td>
    <td>
<pre lang="css">
.evg-h4 {
    font-size: 1.5rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-h5</td>
    <td>Applies styling for a level 5 heading.</td>
    <td>
<pre lang="css">
.evg-h5 {
    font-size: 1.25rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-h6</td>
    <td>Applies styling for a level 6 heading.</td>
    <td>
<pre lang="css">
.evg-h6 {
    font-size: 1rem;
}
</pre>
    </td>
  </tr>
</table>

Note: Use `role="heading" aria-level="<heading level number>"` if you use non-semantic tags like a `div` to ensure that the headings are accessible. 


### Display Classes

Classes for headers outside of the main page content, like jumbotron display headers. Use these classes to increase the font size of headings. 

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-display-1</td>
    <td>Increases size of the heading to the largest level.</td>
    <td>
<pre lang="css">
.evg-display-1 {
    font-size: 6rem;
    font-weight: 700;
    line-height: 1.2;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-display-2</td>
    <td>Increases size of the heading to the second largest level.</td>
    <td>
<pre lang="css">
.evg-display-2 {
    font-size: 5.5rem;
    font-weight: 700;
    line-height: 1.2;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-display-3</td>
    <td>Increases size of the heading to the third largest level.</td>
    <td>
<pre lang="css">
.evg-display-3 {
    font-size: 4.5rem;
    font-weight: 700;
    line-height: 1.2;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-display-4</td>
    <td>Increases size of the heading to the fourth largest level.</td>
    <td>
<pre lang="css">
.evg-display-4 {
    font-size: 3.5rem;
    font-weight: 700;
    line-height: 1.2;
}
</pre>
    </td>
  </tr>
</table>


### Typography Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-text-primary</td>
    <td>Changes text color to the primary color.</td>
    <td>
<pre lang="css">
.evg-text-primary {
    color: #343a40;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-text-secondary</td>
    <td>Changes text color to the secondary color.</td>
    <td>
<pre lang="css">
.evg-text-secondary {
    color: #6c757d;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-text-sm</td>
    <td>Decreases size of text within the main content (non-heading) of the page.</td>
    <td>
<pre lang="css">
.evg-text-sm {
    font-size: .85rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-text-lg</td>
    <td>Increases size of text within the main content (non-heading) of the page.</td>
    <td>
<pre lang="css">
.evg-text-lg {
    font-size: 1.25rem;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-dark-on-light</td>
    <td>Applies a <em>dark text over light background</em> color theme. Use to change the text color in the foreground and the color of the background</td>
    <td>
<pre lang="css">
.evg-element1.evg-dark-on-light {
    color: #343a40;
}
.evg-element2.evg-dark-on-light {
    background-color: #fff;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-light-on-dark</td>
    <td>Applies a <em>light text over dark background</em> color theme. Use to change the text color in the foreground and the color of the background</td>
    <td>
<pre lang="css">
.evg-element1.evg-light-on-dark {
    color: #fff;
}
.evg-element2.evg-light-on-dark {
    background-color: #343a40;
}
</pre>
    </td>
  </tr>
  <tr>
    <td>.evg-link</td>
    <td>Applies styling for text links.</td>
    <td>
<pre lang="css">
.evg-element1.evg-light-on-dark {
    color: #fff;
}
.evg-element2.evg-light-on-dark {
    background-color: #343a40;
}
</pre>
    </td>
  </tr>
</table>


### Other Classes

<table>
  <tr>
    <th>Class</th>
    <th>Purpose</th>
    <th>Example CSS</th>
  </tr>
  <tr>
    <td>.evg-hide</td>
    <td>Class for hiding elements.</td>
    <td>
<pre lang="css">
.evg-hide {
    display: none;
}
</pre>
    </td>
  </tr>
</table>