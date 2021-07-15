---
path: '/campaign-development/email-campaigns-and-templates/third-party-tracking-for-email-campaigns'
title: 'Add Third Party Tracking Parameters to Open-Time Email Campaigns'
tags: ['template', 'email campaigns', 'ote template', 'ote', 'utm parameters', 'utm']
---

Urchin Tracking Module (UTM) parameters are tags that you can add to the end of a URL to track its performance and know its traffic sources. Adding UTM parameters to URLs in your email campaigns can give you much more detailed insights into your campaign's performance. While you can rely on Interaction Studio for analytics and full campaign performance reporting, you can still use your existing web analytics system to monitor click-through traffic with your Open-Time Email campaigns. To learn more about how you can combine third-party tracking data with data from Interaction Studio, please speak with your Salesforce account representative.

This article describes how to use UTM parameters with Open-Time Emails. To know more about Open-Time Emails or how to create one, read the [Email Campaigns](https://doc.evergage.com/display/EKB/Email+Campaigns) documentation. This article assumes that you have created or cloned an Open-Time Email campaign, fully tested it, and have the HTML code for the campaign open in front of you.

## Adding UTM Parameters to Your Emails

To add UTM parameters to your emails, do the following.
1. Access the Open-Time Email campaign within the **Campaign Edit** screen in Interaction Studio UI. For detailed instructions on accessing the **Campaign Edit** screen, refer to the [Access the Campaign Edit Screen](https://doc.evergage.com/display/EKB/Capture+an+Open-Time+Email+Campaign+URL#CaptureanOpen-TimeEmailCampaignURL-AccesstheCampaignEditScreen) article in the Interaction Studio Knowledge Base.
1. Click **SIMULATE** > **Show HTML**.
1. Identify the URLs you'd like to track in your email campaign. You'd most likely want to track URLs for item recommendations included in your email template along with any other URLs of importance.
1. Append UTM parameters at the end of each of the identified URLs, starting with and separated by the & symbol.

For example, if the URL of an item in your email is `https://demo.evergage.com/api/dataset/engage/campaign/YnFdJ/N7pF1/1/detail?userId=@@USER_ID@@`, append the UTM parameters to the end of the URL as shown below.

```html
https://demo.evergage.com/api/dataset/engage/campaign/YnFdJ/N7pF1/1/detail?userId=%40%40USER_ID&utm_source=newsletter&utm_medium=email&utm_campaign=lead_generation
```

<div class="alert-blue">

**Note:** 
- It is essential to add the `&` symbol before every UTM parameter in a URL. Excluding the `&` symbol breaks the URL and leads to improper tracking.
</div>

After you've added UTM parameters to all the relevant URLs, you can use the updated campaign HTML with your existing email marketing solution to send your email campaign to recipients and track click-through rates in your web analytics system.

## Example

The following code sample contains HTML for a row of three item recommendations in an Open-Time Email campaign.

<div class="alert-blue">

**Note:**
- This sample HTML is for illustrative purposes only and includes an illustrative dynamic variable `@@USER_ID@@` for `userId`. The dynamic variable your email marketing solution uses may differ.
- The item and item image URLs in your Open-Time Email campaign may require modification depending on your email marketing solution. For more information on capturing and modifying campaign URLs, refer to [Capture an Open-Time Email Campaign URL](/campaign-development/email-campaigns-and-templates/capture-open-time-email-campaign-url).
</div>

```html
<!-- start Interaction Studio Recommendations block (Item Recommendations Row 1) -->
<table cellspacing="0" cellpadding="0" border="0" align="center" width="100%" style="table-layout: fixed;">
    <tbody>
        <tr>
            <td width="100%">
                <div class="evergage-block" style="max-height: 100%; max-width: 100%; position: relative; overflow: hidden; text-align: center;">
                    <div class="evergage-recs" style="display: table; width: 100%; max-height: 100%;">
                      <!--Item 1-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                        <!--Item 2-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                        <!--Item 3-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/3/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/3/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                    </div>
                </div>
            </td>
        </tr>
    </tbody>
</table>
<!-- end Interaction Studio Recommendations block (Item Recommendations Row 1) -->
```

The goal of this example is to track the traffic source of item URLs in the campaign. To do so, we'll append the UTM parameters `utm_source=newsletter`, `utm_medium=email`, and `utm_campaign=lead_generation` at the end of each item URL, after `detail?userId=@@USER_ID@@"`.

The sample campaign HTML after adding UTM parameters to each item URL is as shown below.

```html
<!-- start Interaction Studio Recommendations block (Item Recommendations Row 1) -->
<table cellspacing="0" cellpadding="0" border="0" align="center" width="100%" style="table-layout: fixed;">
    <tbody>
        <tr>
            <td width="100%">
                <div class="evergage-block" style="max-height: 100%; max-width: 100%; position: relative; overflow: hidden; text-align: center;">
                    <div class="evergage-recs" style="display: table; width: 100%; max-height: 100%;">
                      <!--Item 1-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/detail?userId=@@USER_ID@@&utm_source=newsletter&utm_medium=email&utm_campaign=lead_generation">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                        <!--Item 2-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/detail?userId=@@USER_ID@@&utm_source=newsletter&utm_medium=email&utm_campaign=lead_generation">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                        <!--Item 3-->
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/3/detail?userId=@@USER_ID@@&utm_source=newsletter&utm_medium=email&utm_campaign=lead_generation">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/3/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                    </div>
                </div>
            </td>
        </tr>
    </tbody>
</table>
<!-- end Interaction Studio Recommendations block (Item Recommendations Row 1) -->
```

You can now copy over the updated HTML into your existing email marketing solution to send your email campaign to recipients and track the performance of your item URLs in your web analytics system.