---
path: '/web-integration/sitemap'
title: 'Sitemap Development in Interaction Studio'
tags: ['site', 'map', 'sitewide', 'javascript', 'client']
---
In traditional web development, a "Site map" (or "Sitemap") is a term usually used to represent a hierarchical list of pages of a web site. In Interaction Studio, the term **Sitemap** represents the JavaScript configuration through which the Interaction Studio web-channel solution architecture is implemented. An Interaction Studio developer must configure a Sitemap to be able to collect the user interactivity data on all desired website "page types" and support the business requirements to: 
* Develop [web templates](/campaign-development/web-templates) that enable the delivery of [web channel campaigns](/campaign-development) to targeted prospects and customers
* Enrich the company knowledge and data profiles of customers, prospects, products, and site content assets based on website visitor actions
* Enable the development of in-depth behavioral analytics and machine learning capabilities

## Sitemap JavaScript Functionality
The JavaScript configured in an Interaction Studio Sitemap provides the functionality that enables Interaction Studio to identify: 
* **User Profiles:** Who is interacting with the website (known user ID or anonymous user tracking ID), enabling the ability to develop a user profile based on user browsing actions and existing user data drawn from company systems. See [Capturing User and Account Attributes](/web-integration/sitemap/site-map-user-account-attributes) and [User Identity Mapping](/web-integration/user-identity-mapping).
* **Page Types:** Type of page the user is browsing (Home page, Product page, and so on). See [Page Types](/web-integration/sitemap/pagetypes).
* **Attributes:** Detailed data on the attributes of the user and on the business objects that the user is interacting with, such as product or service catalog items or other web content on the site (blogs, articles, and so on). See [Capturing User and Account Attributes](/web-integration/sitemap/site-map-user-account-attributes) and [Attributes](/data-model/attributes).
* **Content Zones:** Areas of each web page type that Interaction Studio can monitor and target for personalized user engagement opportunities. See [Content Zones](/web-integration/sitemap/contentzones)
* **Events:** Types of user activity to listen for that will trigger events to Interaction Studio. 

The JavaScript for an Interaction Studio Sitemap must be configured before development can begin on the Interaction Studio [web templates](/campaign-development/web-templates) that support customer engagement [campaigns](/campaign-development) based on customer and prospect web channel interactions. 

## Sitemap Blueprint 
As part of the Interaction Studio discovery process, a solution architect works with the Interaction Studio customer to understand the business context and determine the web-channel solution architecture that the Sitemap configuration will need to implement. The discovery process should produce a **Sitemap Blueprint** that provides the Sitemap developer with the following information:

* **Site Catalog:** The brands, products, services or other website content such as articles and blogs that Interaction Studio will need to monitor. This includes the specifications of product categories such as "home goods", "camping" and "winter wear" and individual product dimensions such as color, gender, and keyword. 
* **Site Architecture:** Structure of the site and layout of page types. A large online retailer may have hundreds or even thousands of individual Product pages, but the structural layout of each Product page type will be the same. The Sitemap developer must understand the site architecture and the types of pages that require configuration in the Sitemap. 
* **Attribute Model:** Attributes are used in a Sitemap to collect and store additional custom data for each business object type relevant to the business. Business objects that can have associated attributes include users, accounts, catalog items, orders, and line items. Attributes can also include metadata on on the attribute value such as the attribute data origin/provider, sensitivity classification (PII, sensitive, non-sensitive), and refresh date of the attribute value. Attributes provide additional information to Interaction Studio that Campaign developers can then use for targeting, segmentation, and machine learning purposes. An attribute model schema should be developed as part of the discovery process that specifies the individual attribute name/value pairs, associated metadata being used, and the purpose of each attribute.
* **Web Engagement Use Cases:** The Sitemap Blueprint will specify the use cases that explain how the business wants to engage with its prospects and customers through the website, and should include the [Content Zone](/web-integration/sitemap/contentzones) requirements for the site. 
* **Site Events:** The types of actions done by users on the site that should be sent to Interaction Studio for the purpose of segmentation, campaign targeting, or behavioral tracking (machine learning). See [Event API](/event-api) and [Sitemap Implementation](/web-integration/sitemap/sitemap-implementation-notes).

## Sitemap Integration and Configuration
The Interaction Studio Sitemap system is deployed by and executes within a Beacon API and Web SDK developed by Salesforce for Interaction Studio. A Sitemap is integrated to a site by establishing a connection from the site to Interaction Studio through a `<script>` tag added in the site header. 

    <script src = "https://<Interaction Studio Deployment URL>/scripts/evergage.min.js"></script>

Sitemap configuration is done using the **Sitemap Editor** included with the Interaction Studio Visual Editor. Once the Web SDK script tag has been added to a site, the Sitemap Editor can be used to configure the site to support the Interaction Studio web channel solution. Using the Sitemap Editor, a developer can leverage the JavaScript functions provided by the Interaction Studio Web SDK library, and execute that functionality through HTTP requests to the Interaction Studio **Event API**. 

> **Note:** HTTP requests are sent automatically by the Web SDK based on the configuration of the 
> Sitemap. The Web SDK also provides an API to send events by calling a function (Evergage.sendEvent) from a Sitemap listener.   
### Getting Started with Sitemapping
For guidance on getting started with Interaction Studio Sitemapping and the Sitemap Editor, see [Getting Started with Site Mapping](/web-integration/sitemap/site-map-getting-started).
