---
path: '/campaign-development/campaign-template-apis/recommendations'
title: 'Recommendations' 
tags: ['recs', 'api', 'gears']
---

Interaction Studio Recommendations uses a default gear to fetch recommendations based on the current user and selected 
recipe.

All recommendations in gears are ultimately provided by the Recommendations service, which is provided to applicable 
gear component contexts.

This gear allows you to easily add configuration to a Campaign Template, to control what the business user is allowed to
configure to drive recommendations for the template.

***
## Usage

1. Add `import { RecommendationsConfig, recommend } from "recs";` to your Template. If you want to only return Item IDs
   instead of full items, you can optionally import `recommendIdsOnly` instead of `recommend`.
2. Add a `RecommendationsConfig` property to your Template, and determine what to restrict from the business user, if anything:
    * No restrictions:
        ```ts
        recsConfig: RecommendationsConfig = new RecommendationsConfig();
        ```
    * Restrict to specific type of the catalog items you would like to recommend:
        ```ts
        recsConfig: RecommendationsConfig = new RecommendationsConfig().restrictItemType('Product');
        ```
    * Restrict to specific max results:
        ```ts
        recsConfig: RecommendationsConfig = new RecommendationsConfig().restrictMaxResults(4);
        ```
    * Both:
        ```ts
        recsConfig: RecommendationsConfig = new RecommendationsConfig().restrictItemType('Product').restrictMaxResults(4);
        ```
3. Use the `recommend` function to use the configuration property of type `RecommendationsConfig` you added.

***
## Example Templates
```ts
import { RecommendationsConfig, recommend } from "recs";

export class ExampleRecommendationsTemplate implements CampaignTemplateComponent {

    recsConfig: RecommendationsConfig = new RecommendationsConfig().restrictItemType("Product");

    run(context:CampaignComponentContext) {
        return {
            products: recommend(context, this.recsConfig)
        }
    }
}
```
You can also use the `recommendIdsOnly` function to return only the Item IDs in order of relevance:
***
```ts
import { RecommendationsConfig, recommendIdsOnly } from "recs";

export class ExampleRecommendationsTemplate implements CampaignTemplateComponent {

    recsConfig: RecommendationsConfig = new RecommendationsConfig().restrictItemType("Product");

    run(context:CampaignComponentContext) {
        return {
            products: recommendIdsOnly(context, this.recsConfig)
        }
    }
}
```

## Locales
Note that if locales are configured for a dataset, the items returned will be specific to either the user's locale or,
if the user has no locale, the default dataset locale. This means any item that is returned from the recommendations API
will be translated into the user's language, provided a locale item specific to that language exists.