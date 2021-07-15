---
path: '/campaign-development/server-side-campaigns-and-templates/contentzones'
title: 'Server Side Campaigns and Content Zones'
tags: ['template', 'serverside', 'server side', 'server-side', 'content zone', 'content', 'zone', 'contentzones']
---

Server-Side campaigns are different from web campaigns in that Content Zones are not required. You’ll notice that in the server-side template configurator, the content zone selector pane is not present.

Quick details around the differences in how Content Zone(s) are used for web and Server-Side campaigns are as follows:

### Web Campaigns:
The Content Zone selector in a web campaign is required since Interaction Studio is responsible for rendering the campaign.

* A template developer can assign Content Zone(s) to templates to determine where a particular template is allowed to be used on the site by a business user
* A business user targets a campaign to a specific Content Zone on the site and then can apply published templates that were tagged with that Content Zone by their template developer

By linking web templates to specific Content Zone(s), a template developer is ensuring that Interaction Studio will render the experience correctly, as the template was designed to work on that particular part of the site

### Server-Side Campaigns:
In a server-side campaign execution, the client takes responsibility for rendering the response payload returned by Interaction Studio. Since the output of a server-side campaign is simply a data payload, Content Zone selection is not required for template creation or campaign targeting. In a server-side campaign execution, it is up to the client to:

* Determine what data is required to be passed back as part of the campaign response (defined in server-side template)
* Apply the template to a campaign with appropriate targeting logic (Server-Side Campaign)
* Call Interaction Studio’s Event API and take the data that is returned in the campaign response object and render it in the correct location/format

For clients that have implemented the IS web SDK (with content zones defined in the sitemap) and are utilizing Server-Side campaigns to power personalized experiences, Content Zones can be leveraged as a means to tie a server-side campaign to a specific location on the site. Much like how a web campaign will only return if the Content Zone is present on the page, a server-side campaign that utilizes content zone targeting will also only return if the targeted content zone is present in the request. To add a content zone to your server-side template, all that is required is a basic adjustment to the server-side code (the content zone selector pane that is used for web campaigns is not available for Server-Side campaigns). In the server-side code, just add the line `contentZone: “contentZone1” | “contentZone2”...` Adding more than one Content Zone option will create the Content Zone drop-down selector in the business user template config. The number of Content Zones a template is eligible for is up to the template developer. 

Once a Content Zone is added to a Server-Side template, it is important to make sure that your API calling patterns are configured correctly. Specifically, you will need to have `contentZones` included on the source object in the Event API request. A template with a contentZone applied will only return a response if its targeted contentZone is present in the Event API request. Additionally, only one campaign response per contentZone will be returned. This means if you have two campaigns targeting the same Content Zone and the user is eligible for both, only one campaign will return. A business user can easily prioritize which campaign returns by assigning a priority value to the campaign in the campaign configuration screen. As noted by the info icon next to the priority input box (found in the campaign type window of the campaign creation screen), Campaigns with a higher numeric priority will take precedence. If campaigns share a priority value, the campaign that was created first wins.

The ONLY time a Content Zone value in the server-side code is required as part of a server-side campaign is for promotion selection / decisioning purposes. Einstein Decisions requires that a Content Zone or tag is defined in order to filter which promotion assets are eligible to be returned as part of the campaign response. The Content Zone or tag that you want to use as part of the Einstein Decisions template would need to be hardcoded into the server-side template code.

#### Example Server Side Template with Content Zones
```ts
import { RecommendationsConfig, recommend } from "recs";

export class EinsteinProductRecsTemplate implements CampaignTemplateComponent {

    /**
     * Developer Controls
     */

    /**    
     * The below line illustrates how to add content zones or tags to a server-side template. If more than one is added, it will create the content zone drop-down in the business user config.
     */
    contentZone: "home_recs" | "product_detail_recs_row_1"

    @hidden(true)
    maximumNumberOfProducts: 2 | 4 | 6 | 8 = 4;
    
    @hidden(true)
    maxRatingBound: number = 5;

    /**
     * Business-User Controls
     */
    @title("Recommendations Block Title")
    header: string = "Title Text";

    @title(" ")
    recsConfig: RecommendationsConfig = new RecommendationsConfig()
        .restrictItemType("Product")
        .restrictMaxResults(this.maximumNumberOfProducts);

    run(context: CampaignComponentContext) {
        this.recsConfig.maxResults = this.maximumNumberOfProducts;

        return {
            itemType: this.recsConfig.itemType,
            products: recommend(context, this.recsConfig)
        };
    }

}

```

#### Example Event API Request for Server-Side Campaign Leveraging Content Zones
```json
{
    "action": "action",
    "user": {
        "id": "sample user"
    },
    "source": {
        "channel": "Server",
        "contentZones":["home_recs", "product_detail_recs_row_1"]
    }
}
```
