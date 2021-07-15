---
path: '/campaign-development/web-campaign-debugger'
title: 'Campaign Debugger'
tags: ['campaigns', 'config']
---

## Overview
The _Campaign Debugger_ tool gives you the details of all Interaction Studio activity and data on a page, including:
 - Actions
 - Campaigns, both enabled and disabled
 - Item Data

Before you can enable and use the _Campaign Debugger_, your dataset needs the following:
- Interaction Studio Tools Gear installed
- Beacon Version 16
- Interaction Studio Launcher Chrome Extension
- A completed Sitemap

## Usage
To use the _Campaign Debugger_:

1. Navigate to the page you want to test
2. Click the **Launcher Chrome Extension** icon to open the menu
3. Select **Campaign Debugger**
4. If the Interaction Studio JavaScript beacon is not installed on the page, you must enable **Inject SDK**
to use the Campaign Debugger. 
5. If the Interaction Studio JavaScript beacon is installed, and you would like to test another dataset, you 
should force override the SDK. The URL structure should be: 
`https://ACCOUNT_NAME.evergage.com/beacon/ACCOUNT_NAME/DATASET_NAME/scripts/evergage.js`
5. Once the _Campaign Debugger_ has been enabled, refresh the page and click the blue Interaction Studio tab 
on the right side of the page. 

The rest of this article will detail the specific functionalities of the _Campaign Debugger_ including:

1. Campaign Debugging - Allows you to test campaigns and experiences
2. Events - Provides details for the current page event
3. Campaign Responses - Details for the campaigns that did return
4. Campaign Explanations - Explanations for the campaigns that did not return

### Campaign Debugging
At the top of the debugger tool you'll see information regarding the account, dataset, beacon version, as well as an
input box and toggle for testing campaigns and experiences. If you're interested in testing a particular campaign, type
a partial campaign name in this box to see it in the dropdown, even if it's disabled. Once selected,
you're free to begin testing.

#### Campaign & Experience Testing
There are two modes you can test in using the _Campaign Debugger_:
1. **Campaign Testing Mode** - Use to simulate unpublished campaign behavior and appearance on your site
2. **Experience Testing Mode** - Use to test a specific campaign experience
 
##### Campaign Testing Mode
If you want to test a campaign to see how would behave on your site for your user account without publishing it:
1. Ensure the _Campaign Debugger_ is enabled
2. Select the campaign
3. Enable **Testing Mode** using the toggle switch 
4. Click **Apply**
5. You'll be redirected to the same page with the query parameter `evergageTestMessages=[CAMPAIGN_ID]` in the URL. 
This will cause the campaign to act like it's published so you can test changes outside of the campaign editor. 
You will see whichever campaign experience your user account would qualify for. This testing mode adheres to all of 
the rules configured for the campaign and experiences. This means that there is a chance your user account may not be 
eligible to see the campaign so you won't see anything displayed on the screen. This does not mean that the campaign 
is not working, since it may be simply that you didn't qualify to see it either due to campaign or experience rules, or
being added to the campaign or experience control group.

##### Experience Testing Mode
If you want to test a campaign's specific experience:
1. Ensure the _Campaign Debugger_ is enabled
2. Select the experience you want to test from the dropdown
3. Enable **Testing Mode** using the toggle switch 
4. Click **Apply**
5. This will redirect you to the same page with the query parameter `evergageTestMessages=[EXPERIENCE_ID]` in the URL. 
Experience Testing Mode behavior will be different from _Campaign Testing Mode_ behavior because this mode forces your user 
account to see the specific experience you selected in step 2 even though you may not naturally have qualified to see it.
This is because _Experience Testing Mode_ ignores any rules at the campaign and experience levels and forces you into the
default test group (i.e. you won't see the control). While this mode may not simulate the exact behavior of the 
published campaign, it will give you the opportunity to preview aesthetic changes outside of the Visual Editor.

Note: If you don't see the experience with `evergageTestMessages=[EXPERIENCE_ID]` in the URL, there may be some other 
issue with the campaign. For example, recommendations aren't returning from the server and there's nothing to render. 
If this happens, the _Campaign Explanations_ panel may help elucidate further information. See details below.
   

### Events
This section shows the raw event JSON going to the Interaction Studio server. This includes details 
like:
* User agent
* Content zones included on the page
* userId of the campaign viewer

If there is a mapped item on the page, item data will be shown. This data includes:
 - name 
 - description
 - image URL
 - a link to the item in the Interaction Studio Catalog.

### Campaign Responses
This section shows a list of campaigns returned from the server for this event. These are campaigns that were returned
as part of the response and (assuming the template client-side code handled the response properly) rendered onto the page.
The details included are:
 - Campaign name
 - Campaign ID
 - Specific Experience ID
 - Number of templates rendered
 - Whether your user is currently viewing the control or not (Impression Type).

### Campaign Explanations
The last section of the _Campaign Debugger_ shows which campaigns did not make it back from the server for this event. This can
be due to several common issues, including:
- The campaign is disabled and not being actively tested using the _Campaign Debugger_.
- The server-side template code had an error.
- The Content Zone for the campaign is not on this page.
- The user doesn't meet the rule criteria to see the campaign.
- The items or promotions the campaign intends to return are not be available for this particular configuration.
