---
path: '/campaign-development/web-templates'
title: 'Templates'
tags: ['template']
---

Interaction Studio’s template system, composed of Web Templates & Server-Side templates, provides developers with the ability to customize and configure templates to support their business user’s use case needs. A template developer has complete control over each component of the template and can determine which aspects of the campaign experience a business user can manipulate. Once a developer publishes a template for a business user to use in a campaign, it becomes a reusable tool in the business users optimization arsenal that allows a business to test and learn at speed.

While developers can configure custom templates from scratch, Interaction Studio accounts are equipped  with a variety of global templates specifically designed to cover some of the most common and powerful use cases. These templates require minimal edits to leverage out of the box or can act as the starting point for new template creation. The global templates are both great use case accelerators and living/breathing examples of template code in action. Additional information on the global templates [is available here.](https://doc.evergage.com/display/EKB/Work+with+Global+Templates)

## Template Implementation Options

Interaction Studio has two types of templates available for developers to configure:

* **Web Templates:** Web templates consist of four key parts (Server-Side Code, Client-Side Code, Handlebars, & CSS) and are used when Interaction Studio’s Web SDK is responsible for rendering the campaign experience on the client's site. Web templates also leverage the web content zones that are mapped during the website implementation. They determine where a campaign can render on the website.
* **Server-Side Templates:** Server-Side templates only utilize server-side code and are used to simply pass a JSON payload with customer or campaign response data for another system to then take and render/ingest.

## Template Context

The content displayed in templates is driven by data that we refer to as the `context`. The data in the `context` is derived server-side by Interaction Studio from the configuration of the campaign that is using the template. Once the context is derived on the server, it is passed to the client browser, where the client-side JavaScript can choose what to do with the `context`, including joining the `context` with the HTML and rendering.

## Template Components

Information and instructions on how to approach each component of a template are included in the following documents:

* [Template Handlebars](/campaign-development/web-templates/web-template-handlebars)
* [Template CSS](/campaign-development/web-templates/web-template-html-classes-and-css)
* [Template Client JavaScript](/campaign-development/web-templates/web-template-client-javascript)
* [Template Server TypeScript](/campaign-development/web-templates/web-template-server-typescript)

## Global Web Templates
Interaction Studio provides a number of pre-built global templates for common personalization use cases such as hero banners, popups, product recommendations, info bars, and many more. These templates are highly customizable to suit individual needs and can save developers a significant amount of time over creating a new template from scratch. New global templates are being developed and released regularly to meet a variety of web channel campaign use cases.  

The process for using a global web template involves the following steps:

1. Choose the global template that most closely matches your business use case. 
2. Clone the template. 
3. Modify the template code to meet business requirements. 

See [Get Started with Global Web Templates](/campaign-development/web-templates/web-template-global-templates) for developer guidance in completing the above tasks and the best practices for working with global web templates.
