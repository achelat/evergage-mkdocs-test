---
path: '/web-integration'
title: 'Interaction Studio Web Integration'
tags: ['beacon', 'web channel', 'integration']
---

This article introduces the key features and capabilities of Interaction Studio Web SDK which provides the functionality used to support web integrations. For a description of the Interaction Studio web integration process and development planning considerations, see [Plan a Web Integration](/web-integration/web-integration-planning).

## Web SDK
The Web SDK provides the functions, properties and methods that developers can use to collect information about website visitor browsing behavior and to send that information to Interaction Studio, allowing the company to build profiles of their website vistor/user behavior. The JavaScript beacon included with the Web SDK handles event responses containing personalization web [Campaigns](/campaign-development). These Campaigns are developed by business users through the Interaction Studio UI for display on the website to qualified visitors, and leverage [Campaign Templates](/campaign-development/web-templates) to load data, run machine learning, perform A/B testing, and run client-side display logic.

### Key Web SDK Features
* Identity and cookie management for anonymous and named identity tracking
* Web SDK JavaScript beacon for website integration
* Sitemap for data collection and consent management
* Gears extensions to augment Interaction Studio platform capabilities 
* Gears extensions for renderng web campaigns

The remaining sections of this article introduce each of the above features of the Interaction Studio Web SDK.

## Identity and Cookie Management

The Web SDK supports anonymous and named identity tracking using Interaction Studio first-party cookies or by being passed identity information by the website. This system supports the client portion of the Interaction Studio [identity system](/web-integration/user-identity-mapping) by optionally updating client identity with an encrypted & nonced primary identity to support the merging of profile identities across different devices and channels. For complete information on how to specify user identities in Interaction Studio, see [User Identity Mapping](/web-integration/user-identity-mapping).

### Interaction Studio First-Party Cookies
Interaction Studio cookies are prefixed with `_evga_`, and end with a string of characters unique to the dataset to which the cookie belongs. These cookies can be cleared for [consent management](#consent-management-through-the-sitemap) purposes when a user does not consent to cookie tracking.

## Web SDK JavaScript Beacon

The JavaScript beacon provided with the Interaction Studio Web SDK is required for managing the flow of data between the Interaction Studio platform and the Web SDK libraries used in the client integration. The beacon is a light weight JavaScript file that provides script-loading functionality on page load, as well as an event message bus and the ability to bootstrap plug-ins for advanced logic and integration. Site pages that do not contain the JavaScript beacon will have no behavioral tracking or personalization capabilities. See [Validate JavaScript Beacon Deployment](/web-integration/sitemap/validate-beacon-deployment) for instructions on how to confirm whether the JavaScript beacon is present on a site page.


## Sitemap

The Interaction Studio Sitemap system is a configuration-driven integration layer that is deployed by and executes within the Interaction Studio Web SDK. A "Sitemap" is developed to integrate with a specific site using the APIs provided by the Web SDK. These APIs map key business objects of the company into the Interaction Studio system through event capture. This can include page type information and catalog items, supporting the ability to understand user interests against the catalog. Objects can also include items such as products, article content, and categories, and can also include custom dimensions (categorical information about items with custom attributes). See the articles in this site on [Sitemap](/web-integration/sitemap) and [Data Model](/data-model) for more details.

### Consent Management Through the Sitemap

Some users refuse cookie tracking. The Web SDK will only initialize tracking when it is explicitly initialized within the Sitemap. A simple conditional statement can be added to your Sitemap code in order to prevent Interaction Studio from initializing in the event a user does not consent to cookie tracking.

```js
if (consent) Evergage.init().then({
    // sitemap code here
});
```

## Web SDK Gears Extensions

The JavaScript beacon provided with the Interaction Studio Web SDK supports the delivery of custom functionality to augment the client-side functionality of the Interaction Studio Platform through [Gears](/gears) extensions. These extensions are JavaScript files delivered and installed through various Gears which are developed by the Interaction Studio development team. 

## Web Campaign Templates
Gears extensions can also be used in configured [web templates](/campaign-development/web-templates) to deliver campaign and personalization content directly to site code so it may be rendered by the website. Interaction Studio includes a built-in **Handlebars Gear** that supports rendering dynamic content into HTML as well as CSS styles using the [Handlebars](https://handlebarsjs.com) template language.

