---
path: '/event-api'
title: 'Interaction Studio Event API'
tags: ['api', 'event']
---

The Event API allows sources to send event data to Interaction Studio to be processed in the event pipeline, which then returns Campaigns which can be served to the end user. The Event API also allows for authenticated requests, returning internal Campaign data.

> **IMPORTANT!** The Event API should not be used to handle mobile application events. The Interaction Studio [Mobile SDK](/mobile-integration) has separate functionality used for mobile app event processing, with built-in features that are currently unavailable through the Event API.

## Event API Specifications
The [Event API Specifications](/event-api/event-api-specifications) article in this section specifies standard HTTP methods and JSON-formatted objects that can be used in the Sitemap and other Interaction Studio event sources to interact with the Interaction Studio platform.