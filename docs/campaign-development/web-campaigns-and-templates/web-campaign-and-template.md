---
path: '/campaign-development/web-campaign-and-templates'
title: 'Web Campaigns & Templates'
tags: ['campaign', 'web', 'template']
---

As you work to modify existing templates or build out your own custom template, referencing code in the global templates is often a great place to start. Even if they do not map exactly to the use case you are building, the global templates provide template developers with a view into common functions / configurations that might help you solve your use cases. A quick breakdown of some of the key code components used in our global templates is detailed below.

| **Global Template** | **Key Code Components Leveraged in the Template** | Sample Configuration Options to Consider | 
--- | --- | --- |
| Banner with Call-To-Action | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li># of CTA's</li> <li>Text / CTA Location</li> <li> Banner Size</li><li>Banner style (Flat image vs layered)</li></ul>
| Einstein Content Recommendations | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Recommendations API](https://developer.evergage.com/campaign-development/campaign-template-apis/recommendations)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of recommended items (content)</li> <li>Style of recommendations</li> <li>Location of recommendations based on mapped content zones </li></ul>
| Einstein Product Recommendations | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Recommendations API](https://developer.evergage.com/campaign-development/campaign-template-apis/recommendations)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of recommended items (products)</li> <li>Style of recommendations</li> <li>Location of recommendations based on mapped content zones </li></ul> |
| Exit Intent | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>[Rich Text (String Property)](https://developer.evergage.com/interface-config/properties/string)</li> <li>[Display Utils (Client-Side Code)](https://developer.evergage.com/campaign-development/web-templates/web-display-utilities)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of calls to action</li> <li>Text/CTA location</li> <li>Pop-up image style (flat or layered)</li> <li>Pop-up contents (recipe inclusion)</li></ul>
| Exit Intent with Email Capture | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>[Display Utils (Client-Side Code)](https://developer.evergage.com/campaign-development/web-templates/web-display-utilities)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Evg.sendEvent (send value entered in campaign to user attribute) client-side code](https://developer.evergage.com/web-integration/sitemap/site-map-user-account-attributes#setting-user-and-account-attributes-using-event-listeners)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Text/CTA location</li> <li>Pop-up image style (flat or layered)</li> <li>Attribute collected</li> </ul> |
| Google Analytics Segment Push | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>User Segment Reference / Lookup from the Common Gear</li> </ul> | <ul><li>Limited default options due to specificity of template</li></ul> |
| Infobar with User Attribute & Call-To-Action | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>Illustrates how to render a user attribute in a campaign response (Server-Side Code)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of CTA's</li> <li>User attribute leveraged (default is visitor's name)</li> <li>Text & background color/styles</li></ul> |
| Infobar with Call-To-Action | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of calls to action</li> <li>Infobar styling</li> </ul> |
| Redirect | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Evergage.sendStat](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats#sending-campaign-stats-without-the-campaign-stats-gear)</li></ul>| - |
| Slide-In with Call-To-Action | <ul><li>[Decorators](https://developer.evergage.com/interface-config/decorators)</li> <li>[Select Property](https://developer.evergage.com/interface-config/properties/select)</li> <li>[Display Utils (Client-Side Code)](https://developer.evergage.com/campaign-development/web-templates/web-display-utilities)</li> <li>[Flicker Defender](https://developer.evergage.com/campaign-development/web-templates/web-flicker-defender)</li> <li>[Campaign Stats Gear](https://developer.evergage.com/campaign-development/web-templates/web-campaign-stats)</li></ul> | <ul><li>Number of CTA's</li> <li>Text & background color/styles</li> <li>Slide-in location & speed</li></ul> |
