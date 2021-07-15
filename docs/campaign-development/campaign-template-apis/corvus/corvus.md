---
path: '/campaign-development/campaign-template-apis/corvus'
title: 'Corvus'
tags: ['contextual-bandit', 'decisions', 'corvus', 'api', 'gears']
---


The Corvus AI is a service responsible for training the Contextual Bandit which is leveraged in Einstein Decisions. This gear allows
Contextual Bandit arms to be fetched from within the context of a campaign template.

## Usage
Import `ContextualBanditConfig` into any Template and define a config, declaring content zone, max number of
promotions returned (up to 12 are allowed), and explicit fallback promotions to use. A filter on which promotions
are eligible for the bandit is optional.

***
Import the `decide` function to request contextual bandit promotions from within the Template's run method.

`decide(campaignComponentContext, contextualBanditConfig, promotionFilter) => Promotion[]`
***
## Example Template
```ts
import {ContextualBanditConfig, decide} from "corvus";

export class CorvusTemplate implements CampaignTemplateComponent {

    run(context: CampaignComponentContext) {
        const banditConfig: ContextualBanditConfig = {
            contentZone: context.contentZone,
            maxResults: 3,
            fallbackArms: ["arm01"]
        } as ContextualBanditConfig;

        const geoPoint: GeographicPoint = { 
            latitude: context.user.location.geographicPoint.latitude,
            longitude: context.user.location.geographicPoint.longitude 
        } as GeographicPoint;
        const geoFence: GeoFence = { radius: 1000, point: geoPoint } as GeoFence;
        const promotionFilter: PromotionFilter = context.services.promotionCatalog
                .promotionFilter(banditConfig.contentZone).withGeoFence(geoFence) as PromotionFilter;
        return { 
            promotions: decide(context, banditConfig, promotionFilter)
        };
    }
}
```
