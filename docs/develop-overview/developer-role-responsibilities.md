---
path: '/develop-overview/developer-role-responsibilities'
title: 'Developer Role and Responsibilities'
tags: ['overview', 'developer', 'role', 'responsibilities']
---

This article introduces the Interaction Studio developer role and its associated development task responsibilities, as well as related tasks that are performed by business users in the Interaction Studio UI.

## Interaction Studio Developer versus Business User Tasks
Interaction Studio can be configured to support customer engagement campaigns across many different digital communication channels (see **Note**), including Web, Email, and Mobile, and through integrations with other systems such as CRM and other external data systems. Some of this configuration is done through the Interaction Studio UI, while other integration and configuration tasks require the development of code specific to the requirements of the company and its business users. For the purposes of this article and the remainder of the articles in this site, "developer" tasks refer to those tasks that require some kind of code development, while "business user" tasks refer to those which are done by users in the Interaction Studio UI. 

---

**Note:** For background and details on how businesses use channels and develop campaigns in the Interaction Studio UI, see [Channels and Campaigns](https://doc.evergage.com/display/EKB/Channels+and+Campaigns) in the Interaction Studio Knowledge Base.

---

## Interaction Studio Developer Tasks
The Interaction Studio system configuration tasks and the respective responsibilities of developers and business users include:

* **Website integration:** [Web channel campaigns](/campaign-development) and the [web templates](/campaign-development/web-templates) that support them require integration with the company website. This integration is achieved through the Interaction Studio Web SDK and the [Sitemap](/web-integration/sitemap), a part of the Web SDK, that recognizes, processes and sends user interactivity data from the website to Interaction Studio. The individual or team responsible for managing scripts on the company website must deploy the Interaction Studio Web SDK to the designated company website pages. Then, a developer must validate the deployment and configure the Sitemap based on the data processing requirements provided by the business. The articles in the [Web Integration](/web-integration) section of this site provide website integration planning considerations, Interaction Studio Web SDK specifications, and Sitemap configuration requirements.
* **Web template development:** Before business users can develop web campaigns in the Interaction Studio UI,  Interaction Studio developers must create the framework for displaying those web campaigns by developing reusable web templates. The [Campaign Development](/campaign-development) section of this site explains the web campaign development process from the developer perspective and contains the articles explaining the technical requirements for web template development.
* **Real-time event data API integration:** Interaction Studio provides real-time data processing through its **Event API**. The Event API is a REST API that processes and responds to event data sent from the Sitemap or from external systems. The articles in the [Event API](/event-api) section of this site cover the purpose, capabilities and requirements for using the Event API, HTTP request/response specifications, and HTTP request/response examples for common Interaction Studio use cases.
* **Data ingestion system integration:** Interaction Studio supports batch data ingestion and processing from external data system sources through ETL or other batch data processing technologies. The complete requirements for integrating external data systems for batch data ingestion into Interaction Studio are documented in [this section](https://doc.evergage.com/display/EKB/Setup+and+Deploy+ETL+Feeds) of the Interaction Studio business user knowledge base. See [Data Ingestion](/data-ingestion) in this developer site for a brief overview.
* **Mobile app integration:** Interaction Studio can support campaigns for display on mobile devices by using responsive HTML (HTML5) in its normal [web templates](/campaign-development/web-templates) feature or through integration with native iOS and Android mobile apps. See [Interaction Studio Mobile Integrations](/mobile-integration) for an introduction to the capabilities of the Interaction Studio iOS and Android mobile app integrations and links to the separate native mobile app developer documentation websites.
