---
path: '/web-integration/sitemap/pagetypes'
title: 'Page Types'
tags: ['site', 'map', 'page', 'types']
---

Page Types define what pages will be tracked on a site and what data will be tracked.  They define the Content Zones, Events, Templates and catalog data that will be tracked on a page.  A Sitemap is made up of many page type configs and a single config can apply to multiple pages. 

### Page Config Fields

The following table summarizes the fields that a PageConfig accepts.

|Field Name	|Expected Value Type	|Required	|Description	|
|---	|---	|---	|---	|---	|---	|---	|---	|
|name	|string	|**yes**	|The name of the page type. Example: Homepage, Product Detail	|
|isMatch	|() => boolean \| () => Promise\<boolean>	|**yes**	|Function that determines if the current page matches the config	|
|action	|string	|no	|Action that is sent when the config is evaluated	|
|catalog	|CatalogConfig	|no	|Catalog data to be captured	|
|cart   |CartConfig |no |Cart data to be captured   |
|order  |OrderConfig    |no |Order data to be captured  |
|contentZones	|ContentZone[]	|no	|Content zones defined on the page	|
|listeners	|Listener[]	|no	|Event listeners to be bound when config is evaluated	|
|onActionEvent	|(event: ActionEvent) => ActionEvent	|no	|Function to modify the outgoing `ActionEvent`	|

## Common Page Types

Depending on the site, there will be page types that are recommended to map based on the vertical.

### General Page Types

The following table outlines general page types that you may want to define on any site

|Page Type	|Description	|
|---	|---	|
|home	|The site's homepage	|
|blog	|The blog's homepage	|
|blog_detail	|A blog post	|
|search	|The search  page	|
|register	|The register page	|
|sign_in	|The Sign in page	|
|account	|The Account overview page	|
|error_page | An error page. Example: 404 Page |

### Commerce Page Types

The following table outlines page types that you may want to define on a commerce site.

|Page Type	|Description	|
|---	|---	|
|product_detail	|The Product Detail page	|
|brand	|The Brand detail page	|
|department	|Department overview page	|
|category	|Category page, sometimes referred to as Product List Page	|
|cart	|The shopping cart	|
|checkout	|The checkout pages	|
|order_confirmation	|The order confirmation page	|

### B2B Page Types

The following table outlines page types that you may want to define on a B2B site.

|Page Type	|Description	|
|---	|---	|
|product	|A specific product offering	|
|solution	|A group of offerings, high level capability, industry	|
|article	|Details page for Articles. Example: Whitepapers, technical specs	|
|event	|Details page for events. Example: Webinars, Conferences	|
|learning	|Details page for Learning content. Example: Classes, Playbooks, Forums, Documentation	|

### Default Page Type

The following table outlines the naming convention for `pageTypeDefault` in the sitemap.

|Page Type	|Description	|
|---	|---	|
|default	|Any page that isn't explicitly mapped	|