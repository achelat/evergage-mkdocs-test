---
path: '/campaign-development/web-templates/web-template-building-checklist'
title: 'Web Template Building Best Practices'
tags: ['template', 'gears', 'zero flicker', 'campaign stats', 'accessibility', 'cross-browser', 'compatability']
---

This article outlines best practices for building templates that will run optimally on your website.

## Campaign Stats Tracking

If you would like to track campaign stats with Interaction Studio, use the data attributes (`data-evg-*`) in the Handlebars HTML. Note that using these data attributes requires the Campaign Stat Tracking gear installed and enabled for your dataset.

Visit the [Campaign Stats Tracking](/campaign-development/web-templates/web-campaign-stats) article to learn more.


## Deferred Rendering

Use `Evergage.DisplayUtils.pageElementLoaded` to defer rendering of your template until the content zone or target element has loaded on the webpage. Note that this method requires the Display Utilities gear installed and enabled for your dataset.

Visit the [Template Display Utilities](/campaign-development/web-templates/web-display-utilities) article to learn more.


## Zero Flicker Compatibility

In order to prevent "flicker" (where the original website content appears briefly before being replaced by the template), the `apply` function in the Clientside Code must return a Promise that resolves once the template renders. Note that the Flicker Defender gear must also be installed and enabled for your dataset. 

Visit the [Flicker Defender](/campaign-development/web-templates/web-flicker-defender) article to learn more.

## Accessibility Standards

Develop templates with accessibility in mind. Ensure that your template is Web Content Accessibility Guidelines (WCAG) compliant in order to provide access to individuals who make use of assistive technologies like screen readers.

Visit the [Templates Style Guide and Coding Conventions](/campaign-development/web-templates/web-templates-style-guide-and-coding-conventions) article to learn more. Read the two sections on "Accessibility" under "Handlebars HTML" and "CSS".

## Cross-browser Compatibility

Develop your template to be compatible across different browsers as needed. Click on the "Test" button in the template editor to test your template on your website in a new separate browser window.