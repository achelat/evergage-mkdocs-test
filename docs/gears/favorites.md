---
path: '/gears/favorites'
title: 'Favorites Gear'
tags: ['gear', 'favorites', 'campaign', 'service']
---

### Gear Configuration

`evergage-gear.json`

```json
{
  "id": "favorites",
  "name": "Favorites",
  "description": "Feature the user’s favorite brands, categories, or any item type in your catalog in Interaction Studio campaigns.",
  "category": "Audience Insights",
  "version": "1.0",
  "author": "Interaction Studio"
}
```

### Implementation

`Favorites.ts`

```typescript
/**
 * Feature the user’s favorite brands, categories, or any item type in your catalog in Interaction Studio campaigns.
 */
const ONE_MINUTE = 1000 * 60;
const ONE_DAY = 1000 * 60 * 60 * 24;
const DEFAULT_WEIGHTS = {
    Purchase: 40,
    Favorite: 30,
    Cart: 20,
    ViewTime: 5 / ONE_MINUTE, // normalize by converting view time from millis to minutes
    View: 4
};


export function favorite(context: CampaignComponentContext, config: FavoriteConfig) : Item {
    let lookbackDays = config.lookbackDays || 90;
    let start = new Date(Date.now() - (lookbackDays * ONE_DAY));
    if (config.statType) {
        let stats = context.user.itemStatTotalPerItem({
            itemType: config.itemType,
            statType: config.statType,
            start: start
        });
        stats.sort(compareStats);
        return (stats.length >= 1) ? stats[0].itemId : null;
    } else {
        let weights = config.weights || DEFAULT_WEIGHTS;
        let resultsById = {};
        Reflect.ownKeys(weights).forEach(statType => {
            let weight = weights[statType];
            let stats = context.user.itemStatTotalPerItem({
                itemType: config.itemType,
                statType: statType,
                start: start
            });
            stats.forEach(function(stat) {
                let result = resultsById[stat.itemId];
                if (!result) {
                    result = {
                        itemId: stat.itemId,
                        value: 0
                    };
                    resultsById[stat.itemId] = result;
                }
                result.value += stat.value * weight;
            });
        });
        let results = [];
        Reflect.ownKeys(resultsById).forEach(id => results.push(resultsById[id]));
        results.sort(compareStats);
        return (results.length >= 1) ? results[0].itemId : null;
    }
}

function compareStats(a, b) {
    // primary: value/score, descending
    if (a.value < b.value) {
        return 1;
    }
    if (a.value > b.value) {
        return -1;
    }
    // secondary: item ID, ascending
    if (a.itemId < b.itemId) {
        return -1;
    }
    if (a.itemId > b.itemId) {
        return 1;
    }
    return 0;
}

/**
 * The configuration/request for `Favorites`'s `favorite` function.
 */
export class FavoriteConfig {
    itemType: string;
    statType?: string;
    weights?: object;
    lookbackDays?: number = 90;
}

```
