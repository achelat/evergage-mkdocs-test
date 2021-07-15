---
path: '/web-integration/site-impact-approach'
title: 'Interaction Studio Site Impact Approach'
tags: ['javascript', 'client', 'websdk']
---
### Overview

The Interaction Studio Web SDK runs on web pages to both gather relevant information and modify content to provide a personalized experience to the web page’s visitors. This document covers details on the expected behavior of the Web SDK in regards to page load performance.

It is important to note that the client side JavaScript served by Interaction Studio is categorized into three main sections within the same file, in the following order: Web SDK, Beacon Extensions, and Sitemap. While the Web SDK portion alone will be the same for every integration, Beacon Extensions and Sitemap can be different and may have their own special considerations regarding site impact.



### Integration
Downloading and executing Interaction Studio’s JavaScript will inherently impact a site’s performance just by being a new piece of content to load. We minimize the impact by:

1. Serving via a global Content Delivery Network (CDN) with over 40 points of presence around the globe.
2. Utilizing our CDNs option to serve stale, such that even if the SDKs origin server is unavailable or slow, we will continue to serve a cached version of the SDK.
3. Client-side caching, to ensure only the first page load requires downloading of the file.
4. Keeping the Web SDK small, under 50 KBs compressed with gzip, before optional Beacon Extensions and the Sitemap.

While the above points apply to all Web SDK integrations, there are two methods for integrating Interaction Studio into a website: Synchronous and Asynchronous, each with different levels of impact on the site.

##### Synchronous
With a synchronous integration, the Interaction Studio’s JavaScript will download at the point it is present in the page before the browser continues to execute the rest of the page. In doing so, the Flicker Defender Beacon Extension will be able to prevent content that may be personalized from appearing before it can be personalized and tracking can take place as soon as possible.

##### Asynchronous
With an asynchronous integration, Interaction Studio’s JavaScript will download alongside the rest of the page’s content loading. Flicker Defender does not function in asynchronous integrations.

##### Integration Failures
Interaction Studio is configured to use multiple industry standard CDN providers, and in the unlikely scenario that one of them has an outage we can quickly reconfigure Interaction Studio to use another provider. [Uptime statistics can be found here.](http://status.evergage.com)

In the case that Interaction Studio’s CDN was unavailable the request to load the SDK will fail or timeout and no personalization will take place. If the site is integrated synchronously, the page will delay loading until the failure.

### Execution
After the JavaScript is downloaded, it is executed in the order of content (Web SDK, Beacon Extensions, Sitemap). We target that the Web SDK’s JavaScript, as well as calls to initialize the Web SDK via the Sitemap, execute in under 30 milliseconds.

##### Execution Failures
Script failures inside of the Web SDK, Beacon Extensions, and Sitemap are caught and logged, to prevent the scripts from negatively impacting the site’s core functionality. Errors can be tracked within Interaction Studio’s event stream to help identify resolutions in cases where the Sitemap errors prevent personalization.

### Event Requests
After execution, an event will be sent to Interaction Studio’s Event API and its response will contain any campaigns for personalization. Interaction Studio targets 20ms for all server-side execution. End user response time is subject to the performance of the internet connection.

##### Request Failures
These requests have no impact on the page loading, and failures will not be noticed by end users. If Flicker Defender is active, hidden content will redisplay after the configured timeout, with a default of 2.5 seconds from the time the beacon extension first ran. [Uptime can be monitored here.](http://status.evergage.com/)