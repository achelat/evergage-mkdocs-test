---
path: '/web-integration/sitemap/contentzones'
title: 'Content Zones'
tags: ['site', 'map', 'content', 'zones']
---

Content Zones define where Interaction Studio campaigns will be rendered throughout a website.  Interaction Studio templates reference content zones within campaigns.  Content Zones can either be defined globally in the global config or for a specific page type in the page type config.

### Content Zone Fields

The following table summarizes the fields that a Content Zone accepts.

| Field Name | Expected Value Type | Required | Description |
|--- |--- |--- | --- |
| name | string | **yes** | The name of the Content Zone. Example: global\_popup, product\_detail\_recommendations |
| selector | string | no | The CSS selector for the content zone target |

Name is a required field for all content zones while selector is optional.  A content zone without a selector would be available to use in templates that target the glass pane such as a popup.

## Common Content Zones

Depending on the site, there are content zones that are recommended to map to be able to use specific global templates.

The following table outlines Content Zones that, if mapped via the sitemap, will enable you to use specific global templates without additional content zone configuration.

| Content Zone | Global Template | Description |
|--- |--- |--- |
| home\_hero | Banner with Call-to-Action | Content Zone for a hero banner on the home page|
| home\_recs | Einstein Content Recommendations | Content Zone for recommendations on home page |
| product\_detail\_recs\_row\_1 | Einstein Product Recommendations | Content Zone for recommendations on product\_detail page |
| product\_detail\_recs\_row\_2 | Einstein Product Recommendations | Content Zone for recommendations on product\_detail page |
| category\_recs | Einstein Product Recommendations | Content Zone for recommendations on category page |
| cart\_recs\_row\_1 | Einstein Product Recommendations | Content Zone for recommendations on cart page |
| cart\_recs\_row\_2 | Einstein Product Recommendations | Content Zone for recommendations on cart page |
| search\_recs | Einstein Content Recommendations, Einstein Product Recommendations | Content Zone for recommendations on search page |
| order\_confirmation\_recs\_row\_1 | Einstein Product Recommendations | Content Zone for recommendations on order confirmation page |
| order\_confirmation\_recs\_row\_2 | Einstein Product Recommendations | Content Zone for recommendations on order confirmation page |
| error\_page\_recs | Einstein Product Recommendations | Content Zone for recommendations on error page |
| add\_to\_cart\_modal\_recs | Einstein Product Recommendations | Content Zone for recommendations on an Add to Cart Modal |
| global\_popup | Exit Intent Pop-up with Email Capture, Exit Intent Pop-up | Content Zone for global popup |
| global\_infobar\_top\_of\_page | Infobar With Call-To-Action, Infobar With User Attribute and Call-To-Action | Content Zone for infobar at the top of the page |
| global\_infobar\_bottom\_of\_page | Infobar With Call-To-Action, Infobar With User Attribute and Call-To-Action | Content Zone for infobar at the bottom of the page |
| global\_slide\_in | Slide-In with Call-To-Action | Content Zone for slide-in messages with call-to-action |
