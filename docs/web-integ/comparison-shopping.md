---
path: '/web-integration/sitemap/comparison-shopping/'
title: 'Comparison Shopping'
tags: ['comparison', 'shopping', 'comparison shopping', 'commerce', 'commerce gear']
---

Comparison Shopping is a specific type of action which occurs every time a visitor copies a product name to the clipboard and then leaves the page. This activity is considered a proxy for identifying when visitors are looking up comparative prices on another web page. For more information about Comparison Shopping's business uses, please refer to [this article in our Knowledge Base](https://doc.evergage.com/display/EKB/Comparison+Shopping).

---

### Requirements
The sitemap must be functional and have at least one working `pageType`. See our [Sitemap documentation](https://developer.evergage.com/web-integration) for more deails.

---

### Comparison Shopping as a Listener
Although there are other ways to setup a Comparison Shopping action, the Interaction Studio team advises using a [Listener](/web-integration/sitemap/site-map-getting-started#listeners). Below is an example of one way that might be accomplished, using the element containing the product name as a target for the listener:

```js
// definition of Evergage.ComparisonShopping
Evergage.ComparisonShopping = (() => {
    let productNameCopied = false;

    const bindComparisonShoppingCopy = (event) => {
        Evergage.cashDom(event.currentTarget).on("copy.evgComparisonShopping", () => {
            try {
                productNameCopied = true;
            } catch (e) {
                Evergage.sendException(e, "bindComparisonShopping: copy event");
            }
        });
    }

    const bindComparisonShoppingBlur = () => {
        try {
            Evergage.cashDom(window).on("blur.evgComparisonShopping", () => {
                if (productNameCopied) {
                    Evergage.sendEvent({
                        action: "Comparison Shopping",
                        source: {
                            contentZones: Evergage.getCurrentPage().source.contentZones
                        }
                    });
                    productNameCopied = false;
                }
            });
        } catch (e) {
            Evergage.sendException(e, "handleComparisonShopping");
        }
    }

    return {
        handleComparisonShopping: (event) => {
            Evergage.cashDom(window).off('.evgComparisonShopping')
            bindComparisonShoppingCopy(event);
            bindComparisonShoppingBlur();
        }
    };
})();

Evergage.init().then(() => {

    // sitemap
    const config = {
        pageTypes: [
            {
                // ...
                listeners: [
                    Evergage.listener("mouseup.initComparisonShopping touchend.initComparisonShopping", ".pdp-primary-info .product-name", Evergage.ComparisonShopping.handleComparisonShopping),
                ]
            }
        ]
    }

    // ...
});

```
