---
path: '/web-integration/sitemap/item-actions'
title: 'Item Actions'
tags: ['site', 'map', 'item', 'actions', 'event', 'api']
---

The Web SDK property of `itemAction` is used to identify actions users take on catalog items such as "Purchase" or "AddToCart".

Item actions are a separate event property from regular actions (`action`). Item actions contain reserved action names used by Interaction Studio to determine how to treat event data. Interaction Studio will automatically insert the appropriate value for this property when sending events containing catalog data originating from matching a pageType mapped in the Sitemap code. This means that the `itemAction` property should not be manually mapped in your `global` or `pageType` configurations with two notable exceptions. 

When mapping pages with `cart` or `order`, the `itemAction` will have to be specified in your pageType configuration. Typically the `cart` object is mapped only on the cart page itself with the `itemAction` value of, "View Cart" (`Evergage.ItemAction.ViewCart`). Similarly, `order` is usually only mapped on the order confirmation page with the `itemAction` value of "Purchase" (`Evergage.ItemAction.Purchase`).

## Item Actions in the Event API

The item action property value will only need to be manually set when sending events to the Interaction Studio platform using the event API. Only the reserved item action values within `Evergage.ItemAction` can be used. Within the context of the Sitemap, the event API is used to send an event independantly of the page matching event. This is achieved using `Evergage.sendEvent()`. 

An example of an event sent using the API within the Sitemap can be found in our [Example Ecommerce Sitemap](/web-integration/sitemap/examples/ecommerce) within the `"product_detail"` pageType configuration. The relevant code block has been included below. In this example, a listener has been mapped containing an add to cart event that will fire when the user clicks on the DOM element referenced by the provided selector, `".add-to-cart"`.

```js

listeners: [
    Evergage.listener("click", ".add-to-cart", () => {
        const lineItem = Evergage.util.buildLineItemFromPageState("select[id*=quantity]");
        lineItem.sku = { _id: Evergage.cashDom(".product-detail[data-pid]").attr("data-pid")};
        Evergage.sendEvent({
            itemAction: Evergage.ItemAction.AddToCart,
            cart: {
                singleLine: {
                    Product: lineItem
                }
            }
        });
    }),
],

```

## Reserved Item Action Values

The possible values for item action are all contained within the `Evergage.ItemAction` object. All values assigned to the `itemAction` property should be referenced from the `Evergage.ItemAction` object in the same way as the add to cart event detailed in the previous section. The possible values for item action are all contained within the `Evergage.ItemAction` object, which is detailed in our typeDoc [here](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/websdk/docs/enums/_evergage_d_.itemaction.html).	The possible values for `itemAction` are all contained within the `Evergage.ItemAction` object, which is detailed in the Typedoc [here](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/websdk/docs/enums/_evergage_d_.itemaction.html). All values assigned to the `itemAction` property should be referenced from the `Evergage.ItemAction` object in the same way as the add to cart event detailed in the previous section.

## Special Item Actions
- `Quick View Item` - Similar to `View Item` but stops the timer for the "background" item and starts a timer for the "foreground" item.
- `Stop Quick View Item` - Stops the timer initiated by the "Quick View Item" item action for the "foreground" item and restarts the timer for the "background" item.

`Quick View Item` and `Stop Quick View Item` are used to accurately track view time in cases where a user "views" an item while already viewing a different item. An e-commerce category page that allows users to view individual products in a popup modal might take advantage of these item actions. In this example, the "background" item is the category and the "foreground" item is the product. A new page load will stop both timers.

Example:
```
    config.pageTypes.push({
        name: "Category",
        action: "View Category",
        isMatch: () => {
            return Evergage.cashDom(".page[data-action='Search-Show']").length > 0 && Evergage.cashDom(".breadcrumb").length > 0;
        },
        catalog: {
            Category: {
                _id: Evergage.resolvers.buildCategoryId(".breadcrumb .breadcrumb-item a", 1)
            }
        },
        listeners: [
            Evergage.listener("click", ".quickview", (e) => {
                const pid = Evergage.cashDom(e.target).attr("href").split("pid=")[1];
                if (!pid) {
                    return;
                }
                Evergage.sendEvent({
                    action: "Category Page Quick View",
                    itemAction: Evergage.ItemAction.QuickViewItem,
                    catalog: {
                        Product: {
                            _id: pid
                        }
                    }
                });
            }),
            Evergage.listener("click", "body", (e) => {
                if (Evergage.cashDom(e.target).closest("button[data-dismiss='modal']").length > 0) {
                    Evergage.sendEvent({
                        action: "Close Quick View",
                        itemAction: Evergage.ItemAction.StopQuickViewItem,
                    })
                }
            })
        ]
    });
```
