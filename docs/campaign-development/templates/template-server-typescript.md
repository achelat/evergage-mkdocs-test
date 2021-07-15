---
path: '/campaign-development/web-templates/web-template-server-typescript'
title: 'Template Server TypeScript'
tags: ['template', 'gears', 'config', 'server']
---

The **Serverside Code** tab in the Template Editor is where developers define code in TypeScript that runs on the server before returning a template. While this tab is available on both Web and Server-Side templates, Server-Side templates will only have the **Serverside** Code tab and will not have the **Handlebars**, **CSS**, or **Clientside Code** tabs. Additionally, Server-Side templates do not have a content zone to target; you can only leverage them in the corresponding Server-Side Campaigns.

## Template Context Schema and Code Execution

The content displayed in a template is driven by data that we refer to as the context. The Server-Side code (written in TypeScript) is responsible for defining this context and serves two primary purposes:

1. Define the template interface configuration schema, which constructs the shape of the template's context
2. Executes code on the server that allows for the population of the context with additional data available in Interaction Studio and any enabled Gears extensions

The template's `context` is a payload that returns from the server-side containing the necessary catalog and user data to render a campaign. In a Server-Side template, Interaction Studio can issue this data as a JSON payload. In a web template, the `context` derived on the server is transmitted to the client browser. In the browser, the client-side JavaScript can choose what to do with the `context`, including using the values from `context` to dynamically build the HTML from the template's **Handlebars** tab and rendering logic defined in the **Clientside Code** tab.

## Template Class Structure

A template is a class that extends `CampaignTemplate` and implements a single `run` method.

```ts
export class ExampleTemplate implements CampaignTemplate {

    @title("Example Field")
    header: string;

    run(context:CampaignComponentContext) {
        return {};
    }
}
```

## Template Interface Config

The properties of the template's class are used to generate a configuration UI that is displayed to a business user when working in the campaign editor. The business user fills out the template's config inputs in the Campaign Editor to contextualize the template.

Examples of items a template developer might expose to a business user in the template config include:

+ Text input fields for experience messaging
+ Recipe dropdown selectors for ML-driven recommendation campaigns
+ Display timing logic
+ Styling (text color, image background, font, etc.)

For more information on the configuration system and all possible UI inputs, refer to the [Interface & Template Configuration](https://developer.evergage.com/interface-config/decorators) documentation.


## Content Zones

A content zone is effectively a "hot spot" for personalization. When configuring a template, the template developer defines the content zones where the template is eligible for use through the **Content Zones in Template** selector in the bottom left corner of the Template Editor. A business user can then only use a template for the area on the site that it was designed for. If a template has more than one content zone assigned (think of a Recommendations row that might exist on many pages), the business user can select which content zone they intend to target in the actual campaign.

Content Zones are defined and mapped as part of the sitemap. A content zone can contain a CSS selector, which directly points to an element on that page. However, since a CSS selector is not required, the content zone can instead be used as a "logical" or "global" target on a page. For example, if a page is rendering popups that appear on top of the page, this is not linked to any specific CSS selector. If you would like to add additional content zones after the initial implementation, you can easily add them via the sitemap editor in the Interaction Studio visual editor. Adding content zones via the in-app Sitemap Editor **WILL NOT** allow them to appear as a selection option in the template editor.

<div class='alert-blue'>

**Important:** Server-Side templates do not have a content zone selector in the template editor screen as server-side campaigns do not need content zones for campaign targeting. However, you can manually configure content zones on server-side templates to tie server-side campaigns to specific locations on the site. For more information on content zones with server-side campaigns, see [Server-Side Campaigns and Content Zones](https://developer.evergage.com/campaign-development/server-side-campaigns-and-templates/contentzones#server-side-campaigns). 
</div>

## Run

The `run` function executes on Interaction Studio servers when the template prepares to construct a JSON response. Here, you can manipulate the values of the context or add additional values not configured by the end-user.

```
run(context: CampaignComponentContext) => Object)
```

The object returned by the `run` function is merged with the context from the configured template. The return value of the `run` function will overwrite any fields from the template that share the same key.

```
import { getDataFromGear } from "mygear";

export class MyTemplate implements CampaignTemplate {

    exampleField: string = "default value";

    run(context:CampaignComponentContext) {
        return {
            additionalData: getDataFromGear(this.exampleField);
        };
    }
}
```


### this Object
You can access any properties defined in the head of the class by prepending the variable with `this`. All properties defined will be added to the template's local instance of `this` in the `run` function. Any parameters created can be accessed for processing in the `run` function.


### Context Object

The `context` object is an instance of `CampaignComponentContext`, the primary way to interact with Interaction Studio properties exposed to templates. The `context` object allows the template author to access the properties defined in the class. For more information on the `context` object, refer to the [CampaignComponentContext](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/campaigncomponentcontext.html) documentation. The `context` object also allows access to the `user` object, including user attributes and services like `recommendations` if enabled (`context.services.recommendations`).

All properties of the template's class are used to generate a configuration UI. This configuration UI is then displayed to a business user when selecting the template for use in an Interaction Studio campaign. The business user then completes the template's configuration inputs in the Campaign Editor to contextualize the template.

For more information on the configuration system and all possible UI inputs, refer to the [Interface & Template Configuration](/interface-config) documentation.


## Server-Side TypeScript Example

The following code sample is a server-side template that allows business users to configure a comma-separated list of user attributes to return in the payload. You can configure the list of attributes to return in the campaign that utilizes the template.

```ts
export class NewTemplate implements CampaignTemplateComponent {
 
   @title("User Attributes To Retrieve")
   @subtitle("Contents must be a Comma separated list")
   userAttributes: string;
 
   run(context:CampaignComponentContext) {
      
       let attrs = this.userAttributes.split(",")
       let attributesToSend = {}
       attrs.filter((value) => {
           return context.user.attributes.hasOwnProperty(value);
       }).map((attributeName) => {
           attributesToSend[attributeName] = {
               value: context.user.attributes[attributeName]
           }
       });
 
       return {
           attributes: attributesToSend
       };
   }
}
```
