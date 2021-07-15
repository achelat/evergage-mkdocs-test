---
path: '/web-integration/web-integration-planning'
title: 'Plan a Web Integration'
tags: ['beacon', 'web channel', 'integration']
---

Combined with helpful tips and tricks, the steps in this document reflect how our internal development teams approach implementations. Follow these steps in the correct order as guidance for planning your Interaction Studio web integration.

## Step 1: Implementation Planning

### Discovery

During the Interaction Studio discovery process, a solution architect works with the Interaction Studio customer to understand the business context for the Interaction Studio web implementation, any web campaign requirements, and the web-channel solution architecture to implement. Ideally, the implementor will also come away from the discovery process with a deep understanding of the website's structure and what data is available for collection by Interaction Studio.

The following list details all the tasks the implementation team carries out during this process. 

* Establish the business context and goals of any web campaigns, along with requirements for the associated web templates. For specific technical documentation about Campaigns and Templates, see the [Web Templates](/campaign-development/web-templates) documentation on this site and the [Web Campaigns and Templates](https://doc.evergage.com/display/EKB/Web+Campaigns+and+Templates) documentation in the Interaction Studio business user knowledge base.

* Identify the identity attributes necessary for your implementation. Take careful consideration when deciding what identities to use; since identity setup is permanent, it has significant implications for your deployment. Refer to the documentation linked below for detailed information about how the identity system functions and how you can configure it for use with the web SDK.

    * [Identity Management](https://doc.evergage.com/display/EKB/Identity+Management)
    * [Configure Identity Types and Attributes](https://doc.evergage.com/display/EKB/Configure+Identity+Types+and+Attributes)
    * [Configure the Identity System for the Web SDK](https://doc.evergage.com/display/EKB/Configure+the+Identity+System+for+the+Web+SDK)

Additional considerations:

* If the client is implementing multiple sites on one account, our [Multisite Implementation Strategy](/web-integration/multisite-strategy) documentation will be helpful.
* If the site is a Single Page Application (SPA), please refer to the [Single Page App Handling](/web-integration/sitemap/sitemap-implementation-notes/#single-page-app-handling) documentation.

### Blueprint

The [site mapping blueprint](/web-integration/sitemap#sitemap-blueprint) is all about planning. A sitemap blueprint is a skeleton for the structure of your sitemap. It serves as a guide for the developer writing the sitemap code and a reference for those unfamiliar with a particular implementation. In the sitemap blueprint, you will define the page types to be mapped, how the platform should treat them, what relevant data is available on each page chosen for mapping, and where individual values to be collected are found within the site. 

Creating a good sitemap blueprint is the part of the implementation process that is key to developing the data model. Ensure that the decisions made during blueprinting make sense from a technical standpoint. For more information about data modeling, see the [Data Modeling](/data-model) documentation.

It's also essential to keep the client's use cases in mind while developing your blueprint to ensure you create a data model capable of supporting those cases. Additionally, creating the blueprint document should be a collaborative effort; it's crucial to involve both a business user and a technical resource familiar with the product throughout this process. 

Once the blueprint has the overall sitemap structure in detail, we recommend adding to the blueprint where each piece of data to be collected can be found on the client's website.

Ensure that you call out all identities needed for this implementation in your blueprint. Finally, get your technical resource to validate whether the values for identity attributes are available on the site and/or through ETL and that all of the identity types you are planning to configure support your use cases. 

## Step 2: Sitemap Development

You can start the Interaction Studio web integration by deploying the [JavaScript web beacon](/web-integration#web-sdk-javascript-beacon) on all pages that Interaction Studio will monitor. Typically, the team or individual responsible for deploying software to the company website carries out this task. 

[Validate the JavaScript Beacon deployment](/web-integration/sitemap/validate-beacon-deployment), either before or during the sitemap development process. Next, develop the sitemap configuration for all page types and test the sitemap data ingestion to ensure that it meets the anticipated web campaign requirements. For more information on the sitemap development process, see [Sitemap Development](/web-integration/sitemap).

### Use a Test Dataset During Development

Leveraging a test dataset to hone your sitemap allows you to experiment during the early stages of the implementation process. It helps ensure that any data captured while working on a sitemap does not contaminate the data in your production environment., Any data captured in your production dataset will be factored into the catalog structure, campaign targeting logic, and machine learning recipe training. Hence, it is essential to avoid polluting your production dataset with unwanted data elements. Most importantly, editing and revising sitemap code directly in production can lead to incomplete and unvalidated code running live on the client's site.

Creating and using a test dataset is a simple but critical measure to ensure safe sitemap iteration and a pristine production environment. You can create a test dataset by referring to the instructions listed in the [Setup and Manage Datasets](https://doc.evergage.com/display/EKB/Set+up+and+Manage+Datasets) documentation.

**Note:** Once you create a new dataset, you will not be able to delete it.

### Use the Interaction Studio Launcher to Work on Your Test Dataset with the Sitemap Editor

First, [Validate the JavaScript Beacon Deployment](/web-integration/sitemap/validate-beacon-deployment) to check whether the site already has the JavaScript beacon installed. Then, install the Interaction Studio Launcher by referring to the [Install and Use the Interaction Studio Launcher](https://doc.evergage.com/display/EKB/Install+and+Use+the+Evergage+Launcher#InstallandUsetheEvergageLauncher-InstalltheEvergageLauncher) documentation. When using your test dataset and viewing the site in your browser, the option you enable on the Interaction Studio Launcher changes based on the presence of the JavaScript beacon. If the beacon is live on the site, enable **Force Web SDK URL**. If the beacon is not live on the site, enable **Inject Web SDK**. For more information on each of these scenarios, refer to the [Interaction Studio Launcher](https://doc.evergage.com/display/EKB/Install+and+Use+the+Evergage+Launcher#InstallandUsetheEvergageLauncher-InjectingtheSDK) documentation. 

Throughout the development process, remember to validate the events sent by your sitemap using all of the methods outlined in the [Sitemap Event Validation](/web-integration/sitemap/sitemap-event-validation) documentation. Doing so will ensure that you identify any timing issues, errors, or unexpected behavior caused by your sitemap code as they occur.

If you are capturing any data from a form as part of your sitemap, remember that it is best to collect form entry data upon form submission _only_. Doing this helps ensure that you are only collecting accurate data intended to be submitted by the user. It also ensures that your data has undergone and passed the same validation processes used to collect these values natively on the client site. 

Finally, ensure that all identities are configured appropriately according to the blueprint. To know more about configuring identity types and attributes, see [Configure Identity Types and Attributes](https://doc.evergage.com/display/EKB/Configure+Identity+Types+and+Attributes).

### SEE ALSO

* [Sitemap Development in Interaction Studio](/web-integration/sitemap)

* Example Sitemaps:

    * [Example Financial Services Sitemap](/web-integration/sitemap/examples/financial-services)
    * [Example Ecommerce Sitemap](/web-integration/sitemap/examples/ecommerce)
    * [Example B2B Sitemap](/web-integration/sitemap/examples/b2b)

## Step 3: Sitemap Validation

Once the implementor feels like they are in a good spot with their sitemap code, they should review their progress alongside the [Best Practices for Data Capture Using Site Mapping](/web-integration/sitemap/sitemap-data-capture). Then, after reviewing these practices and checking that their code works as intended, they can pass their code off for the next phase of code review within their own organization and make any adjustments based on feedback provided.

## Step 4: Moving an Approved Sitemap to Production

**Important:** If the client's site already has the beacon corresponding to the production dataset integrated into their site, adding a sitemap code to that dataset immediately makes the implementation live and data collection begin. Ensure that the client is ready for this step before proceeding.

This process involves ensuring that the production dataset is configured in the same way as the test dataset you have been using to ensure everything works as expected.

* Reproduce catalog setup from the test dataset in the production dataset.
    * The catalog in the production dataset can be maintained throughout the development process to reflect the Catalog as it gets finalized in your test dataset.
* Paste the completed sitemap code into the production dataset. Make sure you are saving your sitemap code through the web **Sitemap Editor** accessed via the **Visual Editor**.
* Reproduce all recipes and segments created in the test dataset to support your use-cases and/or reporting efforts.
* Configure any third-party integrations from the test dataset in production.
* Configure any feeds from the test dataset in production.
* If development of campaigns and templates began in a test dataset, the following steps may also be required.
    * Export templates from the test dataset and import them into production.
    * Reproduce all campaigns created in the test dataset.

## Step 5: Create Templates and Campaigns

**Important:** As a best practice, complete the sitemap and move it to production before creating any templates or campaigns. Ideally, all of your campaigns and templates will be created only once directly in the production dataset.

Configure and test the web templates required to support the anticipated web campaigns. For guidance, refer to articles in the [Campaign Development](/campaign-development) section of this site and the [Web Campaigns and Templates](https://doc.evergage.com/display/EKB/Web+Campaigns+and+Templates) documentation in the Interaction Studio business user knowledge base.

Once you have developed your campaigns, debug them using the [Web Campaign Debugger](/campaign-development/web-campaign-debugger). You may also want to watch the [Event API Responses](/event-api/event-api-response) directly in the **Network** tab of your browser’s developer tools.

If work on campaigns and templates in the production dataset needs to begin before the sitemap is complete, you _can_ create campaigns and templates within your test dataset. However, be mindful of the following.

* Cloning a dataset does not carry over data, campaigns, or templates.
* If templates are built on a test dataset, you will have to individually export and then import each template from the test dataset into the production dataset before you go live with them.
* Campaigns built on the test dataset will have to be reproduced manually on the production dataset.

Both templates and campaigns will rely on the sitemap in some way to function properly. Without a completed sitemap, it can be difficult to develop a template that will work long-term, considering that the sitemap is still in flux. Recipes and segmentation depend on the proper implementation of the sitemap based on the discovery and blueprinting processes.
