---
path: '/web-integration/sitemap/sitemap-implementation-notes'
title: 'Sitemap Implementation'
tags: ['site', 'map', 'sitewide', 'javascript', 'client']
---

The Interaction Studio Sitemap system is a configuration driven integration layer that is deployed by and executes within the Interaction Studio Web SDK. A Sitemap is developed to integrate with a specific site using the Web SDK, which map key business objects into the Interaction Studio system through event capture. This can include page type information and catalog items, supporting the ability to understand user interests against the catalog. It includes items such as products, article content, and categories, and can also include other user-specified dimensions (categorical information about other items with attributes that are entered by a user in the Interaction Studio UI). See [Sitemap Overview](/web-integration/sitemap) for an introduction to the Interaction Studio Sitemap feature and [Data Model](/data-model) for details on the Interaction Studio data model.

---

#### Article Topics
This article covers the following Sitemap implementation topics:
* [Sitemap Configuration Type Definitions](#sitemap-configuration-type-definitions)
* [Sitemap API](#sitemap-api)
    * [Resolvers](#resolvers)
    * [Utility Functions](#utility-functions)
* [Single Page App (SPA) Handling](#single-page-app-handling)
* [Sitemap Localization Implementation](#sitemap-localization-implementation)

---

#### Example Sitemaps

See [example sitemaps here](/web-integration/sitemap/examples) 

---

## Sitemap Configuration Type Definitions
A `SiteMapConfig` is comprised of three separate configurations: `global`, `pageTypes` (array), and `pageTypeDefault`. These configuration types are defined as follows:

* **Global:** Contains the objects that should be monitored on all page type configuration objects and the default page configuration (if defined).
* **Page Types:** Contains the objects used to detect/match each different page type to be monitored on the site. 
* **Default Page Configuration:** If defined, the objects configured in a `DefaultPageConfig` will match when all `PageConfig` objects in the `pageTypes` array resolve to `false`. 

The following sections describe how the above Sitemap configuration types are processed and provides additional information on each configuration type.

### SiteMapConfig Type Processing
When a `PageConfig` inside the `pageTypes` array matches a page on the site, or if the `pageTypeDefault` matches as described above, the matched config will merge with anything defined in `GlobalConfig`. The merge for individual attributes occurs in the following manner:

* `locale` :  the matched `PageConfig` or `DefaultPageConfig` takes precedence
* `contentZones`: concatenate
* `listeners`: concatenate
* `onActionEvent`: the function for the matched `PageConfig` or `DefaultPageConfig` executes first, followed by the function for the `GlobalConfig`


The result of this merge will then be evaluated as expected.

```
{
    global: GlobalConfig,
    pageTypeDefault: DefaultPageConfig,
    pageTypes: PageConfig[]
}
```
---
### Global Config

Defines what additional data will be tracked if a `PageConfig` or `DefaultPageConfig` matches for a specific page

```
{
    locale?: string | () => string,
    contentZones?: ContentZone[],
    listeners?: Listener[],
    onActionEvent?: (event: ActionEvent) => ActionEvent
}
```
---

### Page Types
Page type configurations use a `pageTypes` array to declare how to detect that a given URL or loaded page is one of the known
page types. Each page type config must contain an `isMatch` property declaring the function that will be used to detect that page type.
Additionally, individual page type configurations may declare [Content Zones](/web-integration/sitemap/contentzones) that represent key parts of a page designed
for personalization adjustments. The configs may also declare other event listeners for a given page type, such as an "add to cart" listener. Page types may map to more than a single page. For example, the product display page (PDP) type would have separate URLs for every product in the site, but only a single page type. The **Home** page is an example of a page type that would have only a single instance of a page. For additional documentation on `pageTypes` [click here](/web-integration/sitemap/pagetypes/).


#### PageConfig

Defines what data will be tracked for a specific page

```
{
    name: string, // required
    isMatch: () => boolean | () => Promise<boolean>, // required
    locale?: string | () => string,
    action?: string,
    catalog?: CatalogConfig,
    cart?: CartConfig,
    order?: OrderConfig,
    contentZones?: ContentZone[],
    listeners?: Listener[],
    onActionEvent?: (event: ActionEvent) => ActionEvent
}
```

`name`

The page type that gets sent in the event when this config is evaluated.

`isMatch: () => boolean | Promise<boolean>`

The function provided here is evaluated when the sitemap is deciding which `PageConfig` to evaluate. The sitemap will only evaluate the first `PageConfig` whose `isMatch` resolves to true. If no `isMatch` resolves to `true`, then the `pageTypeDefault` config will run.

`locale`

The locale of the page (e.g, `en_US`). Only use if you have catalog localization enabled.

`action`

The name of the action that gets sent in the event when this config is evaluated.

`catalog`

See [CatalogConfig](#catalogconfig)

`cart`

See [CartConfig](#cartconfig)

`order`

See [OrderConfig](#order)

`contentZones`

The content zones defined in the sitemap determine where Evergage campaigns are rendered throughout a website. Evergage templates reference these content zones for targeting within campaigns.

Below is an example of a `ContentZone`

```
{ name: Hero Image, selector: '#hero img' }
```

_Note that `name` is the only required field for ContentZones._

`listeners`

See [Listeners](#listeners)

`onActionEvent(event: ActionEvent) => ActionEvent`

This function will run whenever the sitemap builds an `ActionEvent` and is about to send it to Evergage. This can occur as a result of building a `PageConfig` or calling `Evergage.sendEvent`. Use this to modify the outgoing `ActionEvent`. If this function exists on both the `GlobalConfig` and a matched `PageConfig`, then both will be run, with the function in the `PageConfig` running first, followed by the one in the `GlobalConfig`.

---

#### CatalogConfig

```js
catalog: {
    <ItemType>: {
        _id: string | () => string | () => Promise<string | () => string> // required
        categories?: string[] | () => string[] | () => Promise<string[] | () => string[]>
        <attributes>?: value | () => value,
        dimensions?: {
            <dimension>?: string[] | () => string[] | () => Promise<string[] | () => string[]>
        },
        attributes?: {
            <attribute>?: value | () => value
        }
    }
}
```

`ItemType`

The ItemType is defined in the _Catalog Setup_ section of the Evergage platform. These can refer to items such as products or dimensions.

`_id`

The _id of the catalog item. This is required for an item to be tracked.

`categories`

The categories of the catalog item.

`dimensions`

The dimensions of the catalog item.

`<attributes>`

This is where you can define standard, built-in item attributes such as _id, name, price, etc.

`attributes`

Custom attributes on an item, as defined in the _Catalog Setup_ section of the Evergage platform.

For more details on catalog items and item attributes, refer to the Catalog Setup section of [this Knowledge Base article](https://doc.evergage.com/display/EKB/Catalog+Setup).

#### CartConfig

```js
cart: {
    complete: {
        <ItemType>: LineItem[]
    }
}
```

`ItemType`



### OrderConfig

```js
order: {
    <ItemType>: {
        orderId: string | () => string | Promise<string | () => string>,
        totalValue?: double | () => double | Promise<double | () => double>,
        currency?: string | () => string | Promise<string | () => string>,
        lineItems: LineItem[]
    }
}
```

### LineItem

```js
{
    _id: string | () => string | Promise<string | () => string>
    price: double | () => double | Promise<value | ()=> double>
    quantity: integer | () => integer | Promise<value | ()=> integer>
}
```

##### CatalogConfig utility functions

There is a utility function under `Evergage.util` that is especially helpful for building lineItems.

`buildLineItemFromPageState(quantitySelector) => LineItem`

Returns a LineItem constructed from the data already captured on the page, using the given `quantitySelector` to determine how many of the item are in the lineItem.

--------------------

### Default Page Config

Defines what data will be tracked _only if_ all other `PageConfig`s resolve to `false`

```
{
    name: string, // required
    locale?: string | () => string,
    contentZones?: ContentZone[],
    listeners?: Listener[],
    onActionEvent?: (event: ActionEvent) => ActionEvent
}
```
---

## Sitemap API

The Sitemap API is scoped on the `window` under `Evergage`.

---

`Evergage.initSitemap(siteMapConfig: *SiteMapConfig*) => void`

Initialize the sitemap process, using the provided config. Events will not be sent to Evergage unless this function is called.

---

`Evergage.cashDom(selector: *String*, context) => Cash`

A small library for modern browsers that provides jQuery style syntax to wrap modern Vanilla JS features and select elements from the DOM. For more information on `Cash`, see [the docs on GitHub](https://kenwheeler.github.io/cash/#docs).

---

`Evergage.sendEvent(event: *ActionEvent*)`

Send a custom event to Interaction Studio.

---

`Evergage.getState()`

Get the current page state, including values in the config that have already been resolved.

---

### Listeners

`Evergage.listener(bind: keyof WindowEventMap, selector: string, callback: Function): Listener;`

Create a new listener.

A listener is a way to take some action upon an event happening on a page. It is structured similarly to jQuery's event listeners. Below is an example of a simple `listener`. It binds a click to the banner of a website and fires an event to Evergage.

```js
 Evergage.listener("click", ".header-banner", () => {
     Evergage.sendEvent({action: "Banner Click"})
 })
```

Here is an example of an email capture, using a custom callback function:

```js
Evergage.listener("submit", "form#subscribe", (e) => {
    const userEmail = Evergage.cashDom(e.target).find("#email").val();
    if (typeof userEmail === 'string' && /^.+@.+\..+$/.test(userEmail)) {
        Evergage.sendEvent({
            action: 'Email Capture',
            user: { 
                attributes: { emailAddress: userEmail }
            }
        });
    }
})
```

---

### Resolvers

`Evergage.resolvers.<resolver>`

Resolvers return functions so that they can be invoked _after_ a page has been matched. When used in a `PageConfig`, the returned functions do not need to be invoked in order to get the final value - the underlying sitemap code will handle that logic.

The transform functions are callbacks designed to evaluate or modify the resolved value.

`fromSelector(selector: string, transform?: (string) => string) => () => string`

Evergage.cashDom(selector).text()

Returns the text of the first instance of the selector in the DOM.

---

`fromSelectorMultiple(selector: string, transform?: (string[]) => string[]) => () => string[]`

[Evergage.cashDom(selector).eq(0).text(), Evergage.cashDom(selector).eq(1).text(), ...]

Returns the text of all instances of the selector in the DOM in an array.

---

`fromSelectorAttribute(selector: string, attribute: string, transform?: (string) => string) => () => string`

Evergage.cashDom(selector).attr(attribute)

Returns the attribute of the first instance of the selector in the DOM.

---

`fromSelectorAttributeMultiple(selector: string, attribute: string, transform?: (string[]) => string[]) => () => string[]`

[Evergage.cashDom(selector).eq(0).attr(attribute), Evergage.cashDom(selector).eq(1).attr(attribute), ...]

Returns the attributes of the of all instances of the selector in the DOM as an array.

---

`fromMeta(metaName: string, transform?: (string) => string) => () => string`

Evergage.cashDom("meta[name='" + metaName + "']").first().attr("content")

Returns the content of the first meta tag with the given meta tag name.

---

`fromItemProp(itemProp: string, transform?: (string) => string) => () => string`

Evergage.cashDom("[itemprop='" + itemProp + "']").first().attr("content")

Returns the content of the first item prop with the given item prop.

---

`fromWindow(windowVar: string, transform?: (string) => string) => () => string`

Returns the value from a nested object. Returns null if the nested object, or some object in the path, does not exist.

---

`fromJsonLd(path: string, transform?: any) => () => any`

Evergage.cashDom("script[type='application/ld+json']").first().text()

If the path is null, the first JSON-LD script is returned after being parsed into a JavaScript object. If a path is provided, then the value of the path from parsed JavaScript object will be returned.

---

`buildCategoryId(selector: string, startFrom?: integer, ignoreLast?: boolean, transform?: (string) => string) => () => string`

Evergage.cashDom(selector).eq(startFrom).text()|Evergage.cashDom(selector).eq(startFrom+1).text()|...

Returns a pipe delimited string using the text of all instances of the selector.

---

`buildCategoryIdAttribute(selector: string, attribute: string, startFrom?: integer, ignoreLast?: boolean, transform?: (string) =>  string) => () => string`

Evergage.cashDom(selector).eq(startFrom).attr(attribute)|Evergage.cashDom(selector).eq(startFrom+1).attr(attribute)|...

Returns a pipe delimited string using the given attribute of all instances of the selector.

---

`fromCanonical(transform?: (string) => string) => () => string`

Returns a canonical URL, if one exists on the page.

---

`fromHref(transform?: (string) => string) => () => string`

Returns the value stored in `window.location.href`

---

### Utility Functions

`Evergage.util.<util>`

The Evergage WebSDK provides a number of utility functions that can be used to help parse data.

`buildLineItemFromPageState(quantitySelector: string) => LineItem`

Returns a LineItem constructed from the data already captured on the page, using the given `quantitySelector` to determine how many of the item are in the lineItem.

---

`extractFirstGroup(regex: RegExp, str: string) => string`

Returns a string of the first group extracted from the provided regex.

---

`getFloatValue(text: string) => number`

Returns a floating point number parsed from the provided string.

---

`getIntegerValue(text: string) => number`

Returns an integer number parsed from the provided string.

---

`getLastPathComponent(url: string) => string`

Returns the last path component for the provided URL. For example `Evergage.util.getLastPathComponent("https://example.example/myExamplePath.html")` returns `myExamplePath.html`.

---

`getLastPathComponentWithoutExtension(url: string) => string`

Returns the last path component without an extension for the provided URL. For example `Evergage.util.getLastPathComponent("https://example.example/myExamplePath.html")` returns `myExamplePath`.

---

`getParameterByName(name: string, url?: string) => string`

Returns the value of the provided parameter name from the provided URL. If no URL provided, the current location.search of the page will be used.

---

`getPathname(url: string) => string`

Returns the pathname of the provided URL.

---

`getUtagFirstForField(fieldName: string) => any`

Returns the first value for the provided field in a `window.utag_data` object.

---

`getValueFromNestedObject(path: string, obj?: object) => any`

Returns the value from a nested object. If obj is not provided, `window` will be used. Returns null if the nested object, or some object in the path, does not exist.

---

`qualifyUrl(unqualified: string) => string`

Returns a qualified URL href from an unqualified relative URL path. If unqualified is null or unprovided, returns null.

---

`removeQueryString(url: string) => string`

Returns a URL without query strings from the provided URL.

---

`resolveWhenTrue.bind(trueFunc: () => any, bindId?: string timeout?: number, checkInterval?: number) => Promise<any>`

Returns a Promise that will resolve when the provided trueFunc evaluates to `true`. If timeout is not provided, a 2000 ms default timeout will be used. If checkInterval is not provided, a 100 ms default interval will be used to check the trueFunc. If the timeout is reached, the check will be unbound. An `unbind` function is provided under `resolveWhenTrue.getBindings()`. If a bindId is provided the unbind function will be mapped to that key, otherwise a new random key will be used.

`resolveWhenTrue.unbind(bindId: string) => void`

When given a `bindId` unbinds that bound `resolveWhenTrue` function.

`resolveWhenTrue.getBindings() => [key: string]: unbindFunction: ()`

Returns a map of currently bound `resolveWhenTrue` functions.

`resolveWhenTrue.clearBindings() => void`

Clears all currently bound `resolveWhenTrue` functions.

---
## Single Page App Handling

Use the following code to reinitialize the beacon and the sitemap when using Evergage on a single page app (SPA).

```js
Evergage.reinit()
```

Executing the above code will have the following effects:

1. Dispatch an `Evergage.CustomEvents.OnInit` event.
2. Reinitialize the beacon with the same settings as the initial page load (e.g. `cookieDomain`).
3. Rerun the sitemap using the same config as the initial page load.
4. Send activity timing for the previous page.
5. Start activity timers for the current page.

The above code should be wrapped in logic to determine when the beacon and sitemap should reinitialize. For example, to reinitialize the beacon and sitemap when the URL changes without a full page load, see the following sitemap.

**Note:** The following sitemap is for illustrative purposes only. It is advisable to consider other ways of using this function that better suits your specific use case and the way your site functions.

```js
Evergage.init().then(() => {

    const config = {
        global: {},
        pageTypes: [
            {
                name: "home",
                isMatch: () => /home/.test(window.location.href);
            }
        ]
    };
    
    /*
        Check for URL change every 2 seconds. If URL has changed, reinitialize beacon and sitemap.
    */
    const handleSPAPageChange = () => {
        let url = window.location.href;
        const urlChangeInterval = setInterval(() => {
            if (url !== window.location.href) {
                url = window.location.href;
                Evergage.reinit();
            }
        }, 2000);
    }
    
    handleSPAPageChange();

    Evergage.initSitemap(config); 
})
```

---

## Sitemap Localization Implementation

Using the localization setting allows a single dataset to handle a site with multiple languages/locales while utilizing the same catalog and sitemap across languages/locale. To use catalog localizations the "Localization of Catalog Metadata Based on Page Locale" setting must be enabled in catalog settings. Using localizations requires sending the locale attribute in an event. In order to send an event with a locale, see the example of a View Item event below. Locale can be set on the `global`, `pageTypeDefault` and individual `pageTypes` in the SiteMapConfig and can be set manually in events tracked through `Evergage.sendEvent`. Locale needs to be set in a specific format of an ISO 639 alpha-2 language code followed by an underscore followed by an [ISO 3166 alpha-2](http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country code.

Some examples of valid locales are:

* "en_US"
* "en_AU"
* "en_CA"
* "zh_CN"
* "en_GB"
* "pt_PT"
* "en_HK"
* "zh_HK"

#### Example: Setting Locale in Global Config

```js
Evergage.init().then(() => {
    const buildLocale = () => {
        let locale = Evergage.CashDom("meta[type='locale']").attr("content");
        return locale;
    }

    const config = {
        global: {
            locale: buildLocale()
        },
        pageTypeDefault: {
            name: "default"
        },
        pageTypes: [
            {
                name: "home",
                action: "Homepage",
                isMatch: () => {
                    return window.location.pathname === "/";
                }
            }
        ]
    }

    Evergage.initSitemap(config);
});
```

#### Catalog Configs in Different Locales

When building catalog configs in a sitemap, different locales can all use the same config. Since the localized attributes are all on the same item in Evergage, when selecting which `_id` to use for a Catalog Item or for a Catalog Item's dimensions, you will want to make sure you select an `_id` that does not change depending on the locale of the page. Failing to do so will create extra Catalog Items and Dimensions in the catalog and split attribution for users between different locales of the site. All item types can be localized.

For products, you will want to make sure that `currency` is captured in the catalog along with price if available. `currency` takes a `ISO_4217` 3 character string currency code as its value and may change based on locale.

```js
    catalog: {
        Product: {
            _id: getProductId(),
            name: getProductName(),
            price: getProductPrice(),
            currency: "AUD"
        }
    }
});
```

#### Order Config in Different Locales

When creating an order config on a dataset that includes locales an additional field in the order config is required to correctly calculate total revenue. `currency` takes a `ISO_4217` 3 character string currency code as its value. If no currency is provided the currency for items in the order object will be assumed to be the default currency set on the dataset.

```js
{
    ...
    order: {
        Product: {
            orderId: "testOrder123",
            totalValue: 120.25,
            currency: "AUD",
            lineItems: {
                _id: string[],
                price: double[],
                quantity: integer[]
            }
        }
    },
    ...
}
```

Some examples of valid currency values are:

* "USD"
* "AUD"
* "THB"
* "EUR"
* "BRL"
* "DKK"
