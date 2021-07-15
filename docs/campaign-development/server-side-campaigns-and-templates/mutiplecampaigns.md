---
path: '/campaign-development/server-side-campaigns-and-templates/multiple-campaigns'
title: 'Multiple Server Side Campaign Responses in a Single Call'
tags: ['template', 'serverside', 'server side', 'server-side', 'campaign', 'response']
---

Multiple campaign responses in a single call is possible. The Event API will return campaign responses for all server-side campaigns where the campaign targeting criteria is met. For example, if you have 3 personalizations on a single page, a client could simply ensure the following to return multiple campaign responses at once:

* All 3 campaigns are published
* A user qualifies for all 3 campaigns

If the above criteria is met, the Interaction Studio Event API will return 3 campaign responses in the single call. The client is responsible for parsing the campaign response data and determining which campaign response to display and where on the page or end system to display. 

As the number of campaigns a client is running increases, it is important to understand that the number of campaigns returned in a single response can impact latency. Factors that can impact latency include:

* Number of campaigns being returned in a single call
* Amount of data being returned by each campaign
* Complexity of the queries leveraged within each campaign (Einstein Recipes, Einstein Decisions, etc.)

A best practice to consider when returning multiple campaigns in a single request is to limit what is returned to what can be visibly rendered to the end user and load campaigns that are below the fold in subsequent calls as they come into view.

Additionally, to ensure that you are only ever returning the necessary campaigns for a response, we strongly recommend validating that your campaigns utilize appropriate targeting logic (like page targeting). Without proper targeting logic, you run the risk of unnecessarily returning more campaigns than required and ultimately negatively impacting overall performance.