---
path: '/campaign-development/web-templates/web-campaign-stats'
title: 'Campaign Stats Tracking'
tags: ['template', 'gears', 'config']
---

### Overview

The Campaign Stats Gear is designed to track impressions, clickthroughs, dismissals and recommended items. There are a few requirements for how the campaign DOM is structured to support tracking stats in Interaction Studio correctly.

### Background

Campaign statistics are necessary for analytics in Interaction Studio, analytics events emitted to external systems, segment creation, campaign targeting and machine learning optimization.


### HTML Specification

Use the following HTML data attributes in order to properly track campaign statistics using the Campaign Statistics gear

| Attribute name         	    | Description                                                                                                                                                                                                                                           	| Use in template handlebars              	|
|------------------------	|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	|-----------------------------------------	|
| data-evg-campaign-id   	    | The id of the campaign. This attribute must be specified for any client-rendered web campaigns.                                                                                                                                                       	| data-evg-campaign-id="{{campaign}}"     	|
| data-evg-experience-id 	    | The id of the experience. This attribute must be specified for any client-rendered web campaigns. By default, any `a` tags nested within an element containing this attribute will have clicks on them tracked.                                                                                                                                                     	| data-evg-experience-id="{{experience}}" 	|
| data-evg-user-group    	    | The group that the user belongs to. This will either be the test or control group.                                                                                                                                                                    	| data-evg-user-group="{{userGroup}}"     	|
| data-evg-clickthrough         | Indicates that this element is clickable. This attribute can be added to any element with no value to indicate that it should generate a clickthrough when clicked. Note that it is unnecessary to add this value to `a` tags that are children of an element with the `data-evg-experience-id` element.                                                                                    	| data-evg-clickthrough                   	|
| data-evg-ignore-clickthrough  | Indicates that this element is clickable, but should not have a clickthrough tracked for it. This attribute can be added with no value to any element or ancestor of the clickable element.                                                                                   	| data-evg-ignore-clickthrough                   	|
| data-evg-dismissal     	    | Indicates that this element is dismissible. This attribute can be added to any element with no value to indicate that it should general a dismissal when clicked.                                                                                     	| data-evg-dismissal                      	|
| data-evg-item-id       	    | The id of the item represented by this element and its child elements. This id (and the type specified by `data-evg-item-type`) must already be tracked in the Interaction Studio catalog. Adding this attribute to an element, along with `data-evg-item-type`, will bind and track clickthroughs. 	| data-evg-item-id="{{id}}"               	|
| data-evg-item-type       	    | The type of the item represented by this element and its child elements. This type (and the id specified by `data-evg-item-id`), must already be tracked in the Interaction Studio catalog. Adding this attribute to an element, along with `data-evg-item-id`, will bind and track clickthroughs.  	| data-evg-item-type="{{itemType}}"               	|


#### Example for tracking a clickthrough (Handlebars)
```html
<!-- Default click tracking using anchor tag -->
<div id="evg-home-hero-promo" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-content">
        <div class="evg-promo">
            {{header}}
        </div>
        <div class="evg-body">
            {{bodyText}}
        </div>
        <a class="evg-cta" href="{{destinationURL}}">
            {{ctaText}}
        </a>
    </div>
</div>

<!-- Click tracking using data-evg-clickthrough -->
<div id="evg-home-hero-promo" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-content">
        <div class="evg-promo">
            {{header}}
        </div>
        <div class="evg-body">
            {{bodyText}}
        </div>
        <button class="evg-cta" data-evg-clickthrough>
            {{ctaText}}
        </button>
    </div>
</div>
```


#### Examples for disabling a clickthrough without tracking a dismissal (Handlebars)
```html
<!-- Directly on the clickable -->
<div id="evg-home-hero-promo" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-content">
        <div class="evg-promo">
            {{header}}
        </div>
        <div class="evg-body">
            {{bodyText}}
        </div>
        <a class="evg-cta" data-evg-ignore-clickthrough href="{{destinationURL}}">
            {{ctaText}}
        </a>
    </div>
</div>

<!-- On parent element of the clickable element -->
<div id="evg-home-hero-promo" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-content" data-evg-ignore-clickthrough>
        <div class="evg-promo">
            {{header}}
        </div>
        <div class="evg-body">
            {{bodyText}}
        </div>
        <a class="evg-cta" href="{{destinationURL}}">
            {{ctaText}}
        </a>
    </div>
</div>
```


#### Example for tracking a dismissal (Handlebars)
```html
<div id="evg-email-capture-popup" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-popup" style="background-image:url('{{imageUrl}}'); color: {{textColor.hex}}; font-family: {{font}}">
        <div class="evg-close" data-evg-dismissal>
       </div>
    </div>
</div>
```



#### Example for a recommendation campaign (Handlebars)
```html
<div id="evg-product-recs" data-evg-campaign-id="{{campaign}}" data-evg-experience-id="{{experience}}" data-evg-user-group="{{userGroup}}">
    <div class="evg-recs-title">
        {{title}}
    </div>
    {{#each items}}
    <div class="evg-item" data-evg-item-id="{{id}}" data-evg-item-type="{{../itemType}}">
        <a href="{{attributes.url.value}}">
            <img src="{{attributes.imageUrl.value}}" alt="{{attributes.name.value}}"/>
        </a>
            <div>
                <span><a href="{{attributes.url.value}}">{{attributes.name.value}}</a></span>
            </div>
        <div>
            ${{attributes.price.value}}
        </div>
    </div>
    {{/each}}
</div>
```

### Use with Handlebars and Clientside Code

When a campaign is returned to the web browser, the user will be in either a "Test" group or the "Control" group. If the user is in a "Test" group, a template's clientside code will execute the `apply()` function. Likewise, if the user in the "Control" group, a template's clientside code will execute the `control()` function. Both functions can return a `Promise`. Once these `Promise`s resolve, Interaction Studio will dispatch an `Evergage.CustomEvents.OnTemplateDisplayEnd` event and campaign statistics will be tracked automatically.

Note that the "Control" experience _does not_ necessarily need to render HTML into the DOM for the Campaign Stats Gear to track a "Control" impression.



### Use without Handlebars and Clientside Code

If your site does not use Handlebars and Clientside Code in an Interaction Studio template to render campaigns, you can still use the Campaign Stats Gear to track campaign statistics.

When you are ready to track an impression for an Interaction Studio campaign, dispatch an `Evergage.CustomEvents.OnTemplateDisplayEnd` event with the following `detail`

```ts
{
    payload: {
        campaign: string,
        experience: string,
        userGroup: string
    }
}
```

See the code snippet below for dispatching an `Evergage.CustomEvents.OnTemplateDisplayEnd` event with an example payload outlined above

```js
document.dispatchEvent(
    new CustomEvent(window.Evergage.CustomEvents.OnTemplateDisplayEnd, {
        detail: {
            payload: {
                campaign: "abc12",
                experience: "def34",
                userGroup: "Test"
            }
        }
    })
);
```

### Control Click Tracking
In order to track clicks in the control group, the data attributes must be added to elements that are either rendered by the campaign, or exist on site and a user in the control group would see. See the code snippet below for adding these attributes in the `control()` function.
```js
function control(context) {
    Evergage.cashDom("SELECTOR").attr({
        "data-evg-campaign-id": context.campaign,
        "data-evg-experience-id": context.experience,
        "data-evg-user-group": context.userGroup
    });
}
```

If you're using a DisplayUtil such as `pageElementLoaded` in the template's `apply` function, then you might want to similarly use it in the `control` function to ensure that campaign stats are being tracked in a similar manner.
```js
function control(context) {
    const contentZoneSelector = Evergage.getContentZone(context.contentZone);
    return Evergage.DisplayUtils
        .bind(`${context.campaign}:${context.experience}`)
        .pageElementLoaded(contentZoneSelector)
        .then((element) => {
            Evergage.cashDom("SELECTOR").attr({
                "data-evg-campaign-id": context.campaign,
                "data-evg-experience-id": context.experience,
                "data-evg-user-group": context.userGroup
            });
        });
}
```

### Sending Campaign Stats without the Campaign Stats Gear
`Evergage.sendStat` accepts a `CampaignStatEvent`.

#### CampaignStatEvent
```ts
{
    campaignStats: CampaignStat[];
}
```

#### CampaignStat
```ts
{
    experienceId: string,
    stat: 'Impression' | 'Clickthrough' | 'Dismissal',
    control: boolean,
    catalog: { [itemTypeKey: string]: string[] }
}
```

See the code snippet below for using `Evergage.sendStat` to send a `CampaignStatEvent` event with recommended items.

```js
const stat = {
    experienceId: "def34",
    stat: "Impression",
    control: false,
    catalog: {
        Product: ["product1", "product2"]
    }
}

Evergage.sendStat({ campaignStats: [stat] });
```

### Sending Campaign Stats to Third Parties

The following documentation outlines how to listen for and how to access the data of campaign stat events from Interaction Studio. This method can be used to format events for third party tracking systems like Google Analytics and Adobe Analytics.

When a stat is tracked, with or without the Campaign Stats Gear, the OnStatSend CustomEvent is fired. The CustomEvent will contain the following `detail`.

```ts
{
    campaignStat: CampaignStat;
    campaignResponse: CampaignResponse;
}
```

`campaignStat` is the actual stat that is being tracked. `campaignResponse` is an object of all of the data returned from the server for the associated campaign in the `campaignStat`. `campaignResponse` has the following structure:

#### CampaignResponse
```ts
{
    campaignId: string;
    campaignJavascriptContent: string;
    campaignName: string;
    campaignType: string;
    experienceId: string;
    experienceName: string;
    experienceSourceCode: string;
    payload: {};
    state: string;
    templateNames: string[];
    type: string;
    userGroup: string;
};
```

See the code snippet below as an example for how to listen for OnStatSend CustomEvent in a sitemap.

```js
const storedCampaignStats = [];
document.addEventListener(Evergage.CustomEvents.OnStatSend, (event) => {

    const isThirdPartyLoaded = false;

    const statType = event.detail.campaignStat.stat;
    const isControl = event.detail.campaignStat.control;
    const campaignId = event.detail.campaignResponse.campaignId;
    const experienceId = event.detail.campaignResponse.experienceId;

    if (isThirdPartyLoaded) {
        // send campaign stat to third party
    } else {
        // store campaign stat to send later when third party loaded
        storedCampaignStats.push({statType, isControl, campaignId, experienceId});
    }

});
```
