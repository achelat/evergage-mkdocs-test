---
path: '/campaign-development/email-campaigns-and-templates/capture-open-time-email-campaign-url'
title: 'Capture an Open-Time Email Campaign URL'
tags: ['template', 'email campaigns', 'ote template', 'ote']
---

Most (if not all) email marketing solutions allow you to embed URLs within content blocks in email templates to either import images and rich media or link to catalog items or item pages. You can use URLs generated by your Interaction Studio OTE campaigns to import item images or link to catalog items within your externally managed email campaigns. You'll still use the Interaction Studio UI to [create new open-time email campaigns](https://doc.evergage.com/display/EKB/Create+an+Open-Time+Email+Campaign). However, you won't need to copy over all the generated HTML into your email template. Instead, you can cherry-pick item and item image URLs from your OTE campaign and embed them in your email template.

This article describes how and where to find URLs within an Interaction Studio OTE campaign to copy into your email marketing system. This article assumes that you have created or cloned an OTE campaign, thoroughly tested it, and have the HTML code for the campaign open in front of you. To know more about viewing the HTML for your OTE campaign, see [Capture the URL](https://doc.evergage.com/display/EKB/Capture+an+Open-Time+Email+Campaign+URL#CaptureanOpen-TimeEmailCampaignURL-CapturetheURL).

## Capture URLs from Campaign HTML

The following code sample depicts HTML generated by an example Interaction Studio OTE campaign.

<div class = "alert-blue">

**Note:**
- In messages that contain more than one content block (such as this sample campaign), the URL for each block differs by one number only.
- The sample code includes the combined code for all the item blocks in the campaign. You can combine all HTML blocks in your campaign by clicking **COMBINE ALL HTML BLOCKS** when viewing the generated HTML in the UI.
- The characters used after `userId` in the sample code are for simulation purposes only. Do not copy these characters into your email marketing solution. 
</div>

```html
<!-- start Interaction Studio promotion block (Jane Dress Free Shipping Promo) -->
<table cellspacing="0" cellpadding="0" border="0" align="center" width="100%" style="table-layout: fixed;">
    <tbody>
        <tr>
            <td width="100%" align="center">
                <div class="evergage-block" style="max-height: 311px; max-width: 660px; position: relative; overflow: hidden;">
                    <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE1/1/detail?userId=@@USER_ID@@">
                        <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE1/1/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Get free shipping on Jane">
                    </a>
                </div>
            </td>
        </tr>
    </tbody>
</table>
<!-- end Interaction Studio promotion block (Jane Dress Free Shipping Promo) -->
<!-- start Interaction Studio Recommendations block (Item Recommendations Row 1) -->
<table cellspacing="0" cellpadding="0" border="0" align="center" width="100%" style="table-layout: fixed;">
    <tbody>
        <tr>
            <td width="100%">
                <div class="evergage-block" style="max-height: 100%; max-width: 100%; position: relative; overflow: hidden; text-align: center;">
                    <div class="evergage-recs" style="display: table; width: 100%; max-height: 100%;">
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/1/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE2/2/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="Item image">
                            </a>
                        </span>
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
<!-- start Interaction Studio Recommendations block (Item Recommendations Row 2) -->
<table cellspacing="0" cellpadding="0" border="0" align="center" width="100%" style="table-layout: fixed;">
    <tbody>
        <tr>
            <td width="100%">
                <div class="evergage-block" style="max-height: 100%; max-width: 100%; position: relative; overflow: hidden; text-align: center;">
                    <div class="evergage-recs" style="display: table; width: 100%; max-height: 100%;">
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/1/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/1/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="">
                            </a>
                        </span>
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/2/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/2/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="">
                            </a>
                        </span>
                        <span class="evergage-rec" style="display: inline-block;">
                            <a href="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/3/detail?userId=@@USER_ID@@">
                                <img src="https://demo.evergage.com/api/dataset/retail/campaign/PTJqe/zIjE3/3/summary.png?userId=@@USER_ID@@" style="max-width: 100%; border: 0;" border="0" alt="">
                            </a>
                        </span>
                    </div>
                </div>
            </td>
        </tr>
    </tbody>
</table>
<!-- end Interaction Studio Recommendations block (Item Recommendations Row 2) -->
```

To copy over URLs from the above sample campaign HTML, do the following.
1. Locate the `<a>` and `<img>` tags for each content block in the message.
1. Copy the URL from within each tag and do the following.
    - Replace the number in the middle of the URL with the corresponding item number in the email template. For example, if you've created a 2x2 grid layout to display four items in your OTE campaign, each item and item image URL is allocated a number based on their position in the grid, starting from 1 to 4. If your email template uses different numbering to indicate the location of items within the layout, replace the number in your OTE campaign URL with the corresponding number in your email template.
    - Replace the value after `?userId=` in the URL with the macro or tag your email marketing solution provides for dynamically inserting your recipient's email address.
1. Insert the modified URLs into your email marketing solution wherever necessary.

## Example Campaign URLs

The following are examples of item (`<a>` tag) and item image (`<img>` tag) URLs captured from the above sample campaign HTML. `<replaceThis>` in these examples indicates parts of the URL you need to modify before embedding the URL into your email marketing solution.

**`<a>` tag or item URL**

```html
https://demo.evergage.com/api/dataset/retail/campaign/mhegt/jsv81/<replaceThis>/detail?userId=<replaceThis>
```

**`<img>` tag or item image URL**

```html
https://demo.evergage.com/api/dataset/retail/campaign/mhegt/jsv81/<replaceThis>/summary.png?userId=<replaceThis>
```
