---
path: '/web-integration/sitemap/sitemap-data-capture'
title: 'Best Practices for Data Capture Using Site Mapping'
tags: ['site', 'map', 'best', 'practice']
---

After reviewing all relevant sitemap documentation, you should only move your sitemap from your test dataset to your production environment  after thoroughly testing and validating it. Additionally, the information in this document may prove helpful to reference throughout each phase of an implementation. As a final validation for working on a sitemap, we suggest working through the following steps.

The checklists laid out in this document are by no means comprehensive. Instead, they are meant to call out common areas where implementors run into trouble while writing sitemap code.

---

#### Article Topics
This article covers the following topics:
* [Validate the Javascript](#validate-the-javascript)
* [Use the browser extension to test the sitemap code](#use-the-browser-extension-to-test-the-sitemap-code)
* [Evergage.sendEvent usage](#evergagesendevent-usage)
* [Single Page App (SPA) sites](#single-page-app-spa-sites)
* [Timing issues](#timing-issues)
* [Data Capture Sources](#data-capture-sources)

---

## _Validate the Javascript_

### Checklist:

* Check for common Javascript errors by reviewing the sitemap code in the sitemap code editor, found in the Interaction Studio Visual Editor, as it identifies and highlights errors in your code. You can also validate your code by visually spot-checking it for errors.

* We suggest utilizing `Evergage.cashDom` for each implementation instead of using jQuery or some other library that the IS platform does not include (such as Knockout). Since the Interaction Studio Beacon includes CashDom (the [cash](https://kenwheeler.github.io/cash/) library), code written using CashDom remains unaffected by updates made to JS libraries found on the client site, thereby allowing for greater stability over time.

* The sitemap should not be manipulating the DOM in any way; that is the function of the Campaigns and Templates system. For more information on using these features on your Web integration, refer to the [Web Campaigns and Templates documentation](https://doc.evergage.com/display/EKB/Web+Campaigns+and+Templates).

* Resolvers should not be used in situations where a direct value needs to be ingested, like when assigning a value to a variable, resolvers are designed to return a function. For example, this is not a proper use of a resolver: `Evergage.resolvers.fromSelector(".someCssSelector")()`.

## _Use the browser extension to test the sitemap code_

* Injecting or forcing the SDK URL to test the sitemap code.

    * [Install and Use the Evergage Launcher](https://doc.evergage.com/display/EKB/Install+and+Use+the+Evergage+Launcher)

    * If you followed the best practices in the [Plan a Web Integration](/web-integration/web-integration-planning) document, you can validate your sitemap code against a test dataset that is available.

* Once your test dataset is running on the site, click through the mapped pages and check to see that the events are sending as expected.

    * Check out our documentation on [Event Validation Using Chrome Browser Developer Tools](/web-integration/sitemap/sitemap-event-validation#event-validation-using-chrome-browser-developer-tools).

### Checklist:

* No errors in network tab for each event

* Mapped attributes are appearing as expected in the events

* Ensure that the [Sitemap Implementation](/web-integration/sitemap/sitemap-implementation-notes) and any [Event API Request](/event-api/event-api-specifications) objects in your code are structured properly and are collecting values of the correct type for each option according to our documentation.

* When collecting `Category` data associated with another item type (`Article`, `Product` or `Blog`), remember that that the structure and case of category ids should be consistent on all catalog items mapped with categories.

* `Category` ID structure should be carefully validated. When mapping the `Category` item type directly, the value of `_id` in the `pageType` should _always_ be supplied as a string. Additionally, a hierarchy of Categories can be constructed by using `|` separated values to denote each level of the `Category` hierarchy. Ideally, these values will be normalized in some way with your sitemap as they are collected. Normalizing these values helps ensure consistency within your catalog. For example, the Interaction Studio platform considers a `Category` with an ID of "Green Hats" different than a `Category` with an ID of "Green hats". In the following example `Category` ID, all of the principles and best practices concerning `Category` ID structure have been utilized: `main-category|sub-cat-1|sub-cat-2`.

* Check whether listeners are binding correctly on each page and are successfully sending events without errors or incorrectly formatted data, as described above. For example, if a sitemap has a click listener that listens for a click on a button to send an add to cart event, ensure that clicking that button sends an add to cart event with all of the appropriate `lineItem` data successfully.

* Validate that `onActionEvent` is being used correctly. While it is possible to do so, we strongly discourage the use of event listeners and/or the use of `Evergage.sendEvent` in `onActionEvent`. However, if you _do_ decide that you must use `Evergage.sendEvent` here, take extra care to make sure that your code includes an exit case since you will otherwise risk the possibility of triggering an infinite loop. Ensure that `onActionEvent` is not being used to overwrite the contents of the event, rather only to manipulate it and/or append additional data. Additionally, `onActionEvent` should always return the ActionEvent, even if no transformations and/or modifications are made. Examples of properly using `onActionEvent` to ingest user attributes can be found in the [Capturing User and Account Attributes](/web-integration/sitemap/site-map-user-account-attributes) page.

* Validate that your code is using supported and correctly formatted fields when mapping Page Types. The supported fields are outlined in the [Page Types](/web-integration/sitemap/pagetypes) documentation.

* Ensure that your mapped content zones are capable of supporting the customer's use-cases. Refer to the [Content Zones documentation](/web-integration/sitemap/contentzones) which outlines many commonly mapped content zones to get some ideas.

## _Evergage.sendEvent usage_

This function is used to send an Event to Interaction Studio. Events sent using this function should directly follow the structure of the Event API requests, as outlined in the [Event API](/event-api) documentation. Please note that, while very similar, the Event API and page types in the sitemap do have different structures.

## _Single Page App (SPA) sites_

For more information about handling SPA page changes, refer to the [Single Page App (SPA) Handling documentation](/web-integration/sitemap/sitemap-implementation-notes#single-page-app-spa-handling). While the documented example on that page is powered by the use of an interval, you can use a different mechanism to monitor the site for page changes at your own discretion.

## _Timing issues_

* Do selectors work in the console or editor, but the sitemap is still unsuccessful in scraping those values when actually sending the event?

    * The location of the data you are scraping probably does not exist at the time the page matches.

    * Solution:

        * Write a [`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) for the `isMatch` function that matches the page type which relies on data not available precisely at page load. The page will be matched when the `Promise` resolves, that resolution being delayed until the source of your data exists in the DOM or `window` object.
        
## _Data Capture Sources_

When deciding on what to capture, first **consider all possible locations where the data is available on the page for that `pageType`**. While testing a new source of data, ensure the method of obtaining the data persists throughout the Page Type and that the data is available when the sitemap executes. For synchronous integrations, this may mean using a Promise to resolve the `isMatch` function when data is available or using Promises for the catalog `_id` field until the available data is available to capture.

### 1. Window Objects

Data found within window objects (i.e. `window.dataLayer`) tends to be more static and reliable, though be careful about using third-party vendor. Sites correctly implemented with Shopify, Magento, and Drupal tend to be more reliable than others.

 
Taken from the [NTO example](/web-integration/sitemap/examples/ecommerce), we are able to extract the product details found inside the eCommerce data object in the following example:


```js
const getProductsFromDataLayer = () => {
    if (window.dataLayer) {
        for (let i = 0; i < window.dataLayer.length; i++) {
            if ((window.dataLayer[i].ecommerce && window.dataLayer[i].ecommerce.detail || {}).products) {
                return window.dataLayer[i].ecommerce.detail.products;
            }
        }
    }
};
```

The `Evergage.resolver.fromWindow` and `Evergage.util.getValueFromNestedObject` functions can be used to safely return a value from a nested object. For example if capturing data from `window.dataLayer[0].ecommerce.detail.products`, if one part of the path is not defined will cause an error. The `Evergage.resolver.fromWindow` function safely navigates the path given to return a value if it exists or null if the nested object, or some object in the path, does not exist.

### 2. Meta Tags

If the data exists within the meta tag and the DOM, Metatag data is a reliable data format. Interaction Studio has a built-in resolver function to retrieve data from meta tags: `Evergage.resolvers.fromMeta().`

As an example, say a hypothetical website selling widgets contains the following meta tag for description:

`<meta name="description" content="The premier place for all your widget needs.">`

In a catalog config, you can retrieve the description content of that tag with the following code: 

`description: Evergage.resolvers.fromMeta("description");`

`description` will now have the string value:

`The premier place for all your widget needs.`

### 3. itemprop

The itemprop attribute is used to add properties to an element on the page, describing the content in the element. Learn more about the itemprop attribute here: https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/itemprop

Interaction Studio has a built-in resolver function to retrieve data from elements with a specified itemprop: `Evergage.resolvers.fromItemProp` This resolver function will return the content of the first matching element with the given item prop.

For example, when trying to capture the rating on a product you may see that a page contains the following element:

`<meta itemprop='ratingValue' content='5'>`

In a catalog config, you can retrive the rating value of that element with the following code:

`rating: Evergage.resolvers.fromItemProp('ratingValue')`

`rating` will now have the value `"5"`

### 4. JSON-LD

JSON-LD is popular way to annotate and describe elements on a page with structured data. Interaction Studio has a built-in resolver function to parse and retrieve data from a JSON-LD element: `Evergage.resolvers.fromJsonLd`. This resolver function will parse the first JSON-LD script on the page. If given a `path`, it will return the value of the path from the parsed JSON-LD object.

For example if a site had the following element:

```html
<script type="application/ld+json">
        {"@context":"http://schema.org/","@type":"Product","name":"Worlds Greatest Widget","description":"The premier place for all your widget needs.","id":"123","brand":{"@type":"Thing","name":"Widget"},"availability":"http://schema.org/InStock"}
</script>
```

In the catalog config, you can retrieve the entire object using `Evergage.resolvers.fromJsonLd()`, or if you only wanted a single element such as `name`, `Evergage.resolvers.fromJsonLd('name')`.

### 5. URLs

A pages URL can contain helpful information that you may want to capture. For example, a `Product` URL can be captured directily from the URL that the user is on using the `Evergage.resolvers.fromHref` resolver function. Some pages may also contain a canonical link element which is the preferred URL of the page. Canonical URLs can be captured using the `Evergage.resolvers.fromCanonical` resolver function.

### 6. Page Source Code

Sometimes the only source of data is the DOM itself. Interaction Studio has several helper functions that can extract the data you need. The `Evergage.resolver.fromSelector`, `Evergage.resolver.fromSelectorMultiple`, `Evergage.resolver.fromSelectorAttribute` and `Evergage.resolver.fromSelectorAttributeMultiple` resolver functions provide for utilities to extract text and attributes from elements in the DOM. For example if a site had the following element:

```html
<span id="product" data-productId="123">Worlds Greatest Widget</span>
```

In the catalog config, you could retrieve the product Id with the following code:

`_id: Evergage.resolvers.fromSelectorAttribute('#product', 'data-productId')`

and you could retrieve the product's name with the following:

`name: Evergage.resolvers.fromSelector('#product')`

For those familiar with [cash](https://kenwheeler.github.io/cash/), `Evergage.cashDom` can also be used to retrieve data on the DOM. You can view the list of available utility functions in Evergage.js in the source menu inside the dev tool.

### 7. Evergage.DisplayUtils

When you do have to use a Promsie to wait for data to be available, the [Evergage.DisplayUtil](/campaign-development/web-templates/web-display-utilities) functions provide methods which are helpful in waiting for specific elements to be on the page. The `Evergage.DisplayUtil.pageElementLoaded` triggers only when the specified element is loaded on the page. For example, if you need to wait for the `#productId` element to be on the page in order to capture the `_id` of a product, you can use the following:

```js
_id: () => {
    return Evergage.DisplayUtil.pageElementLoaded("#productId", "html").then((ele) => {
        return Evergage.resolvers.fromSelector("#productId");
    });
}
```

### 8. Evergage.util.resolveWhenTrue

When you have to to wait for data to be available on the page, [Evergage.util.resolveWhenTrue](/web-integration/sitemap/sitemap-implementation-notes#utility-functions) provides a set of utility functions to bind and unbind checks that returns a promise that resolves when the provided check returns a truthy value. The `Evergage.util.resolveWhenTrue.bind` function also provides a timeout that will reject if the provided function does not resolve in the specified time. For example, if you need to wait for `window.dataLayer[0].productId` to be available on the page to capture the product _id, you could use the following:

```js
_id: () => {
    return Evergage.util.resolveWhenTrue.bind(() => {
        let productId = Evergage.util.getValueFromNestedObject("window.dataLayer[0].productId");
        if (productId) {
            return productId;
        } else {
            return false;
        }
    }, 1000, 50);
}
```

* * *

### 9. Additional Best Practices

* **Avoid Undefined Data:**
    * If your data field is dependent on a resolver, include it as a return inside a function to avoid having undefined data being sent.
* **Practice Console Logging:**
    * Get into a good habit of console logging the data in the sitemap. Just because it was available in Chrome Developer Tools doesn’t mean it will be available at the precise moment that IS sitemap code is executing. Consider the timing when the data is available.
        * Does it load only after a certain action?
        * Is it only available when the user is logged in? 
    * Console Logging should only be used when developing a sitemap. You should avoid including these statements in a live sitemap.
* **Test your data source thoroughly:** 
    * Navigate to the page from a different link 
    * Click back and forth on the browser button to ensure the data persists through the change. 
    * Some pages may display differently depending on the referring source. This is especially true for single page applications.
* **Consider backup plans:**
    * If the data is available in two separate areas, consider using the second source of data as a backup. 
    * When creating custom helper functions, always assume the value is not available and null check everything.
* **Ensure clean data capture:**
    * Trim the values, especially when it comes to IDs. Errant empty spaces can produce duplicate items in the catalog.
    * `Evergage.resolver` functions will automatically trim values for you.
* **Keep your data capture as simple as possible:**
    * When using selectors, prefer IDs and unique classes over relative selectors if possible. Long chains of child selectors or selectors using `nth-child` may not always return the same element on pages of the same type.
    * When using selectors on the DOM, try to reduce the number of selectors down to the minimum required to obtain the data.
    * If the selector is too specific, it may not capture the data the same way on other Page Types.
* **Double check data capture from Meta Tags:**
    * Be careful of Meta tags, some sites do not always update the Meta tags appropriately.
