---
path: '/web-integration/sitemap/examples/financial-services'
title: 'Example Financial Services Sitemap'
tags: ['site', 'map', 'sitewide', 'javascript', 'client']
---

The following code sample depicts sitemap code for an imaginary implementation of Interaction Studio on the Cumulus Finserv website `https://www.cumulusfinserv.com/`. This sitemap code is not exhaustive and maps only key areas valuable for demonstrating an example implementation of Interaction Studio. The primary focus of this sitemap code is to depict how to approach designing a Catalog for websites offering financial services.

<div class="alert-blue">

**Important:** The example sitemap is for reference purposes only. You should avoid copying this code into your Sitemap Editor as it will not produce the results you intended.
</div>

```js
Evergage.init().then(() => {
    const config = {
        global: {},
        pageTypes: [
            {
                name: "home",
                action: "Homepage",
                isMatch: () => /^\/$/.test(window.location.pathname),
                contentZones: [
                    {name: "home_hero", selector: ".hero-inner"},
                    {name: "home_recommendations", selector: ".intro-content"},
                    {name: "home_nav",selector: "body > div.category-menu"},
                ]
            },
            {
                name: "product_detail",
                isMatch: () => Evergage.cashDom("div.container.product-intro").length > 0,
                action: "View Product",
                catalog: {
                    Product: {
                        _id: Evergage.resolvers.fromHref((url) => url.split("/").splice(-1)[0].toUpperCase()),
                        name: Evergage.resolvers.fromSelector("h1"),
                        price: 1,
                        url: Evergage.resolvers.fromHref(),
                        imageUrl: Evergage.resolvers.fromSelectorAttribute(".img-responsive", "src"),
                        inventoryCount: 1,
                        categories: Evergage.resolvers.buildCategoryId(".nav a.current span", null, null, (id) => {
                            return [id];
                        }),
                        dimensions: {
                            ItemClass: Evergage.resolvers.fromSelectorMultiple("li.current a")
                        }
                    }
                },
                listeners: [
                    Evergage.listener("click", ".product-intro .btn.green-btn.btn-med", (event) => {
                        Evergage.sendEvent({
                            itemAction: Evergage.ItemAction.AddToCart,
                            cart: {
                                singleLine: {
                                    Product: {
                                        _id: Evergage.util.getPathname(window.location.href).split("/").splice(-1)[0].toUpperCase(),
                                        price: 1,
                                        quantity: 1
                                    }
                                }
                            }
                        })
                    })
                ],
                contentZones: [
                    { name: "product_detail_cta", selector: ".btn.green-btn.btn-med" }
                ]
            },
            {
                name: "pre_approved",
                isMatch: () => /\/get-preapproved/.test(window.location.href),
                action: "Pre-Approved",
                listeners: [
                    Evergage.listener("click", "#msform", (event) => {
                        if (Evergage.cashDom(event.target).closest(".next").length > 0) {
                            const step = Evergage.cashDom("#progressbar .active").length - 1;
                            if (step > 0) {
                                const email = Evergage.cashDom("#form-application-email").val();
                                let actionEvent = {
                                    user: {},
                                    action: "Get Pre-Approved Form Step " + step + " Submit"
                                };
                                if (email) {
                                    actionEvent.user.id = email;
                                }
                                Evergage.sendEvent(actionEvent);
                            }
                        }
                        else if (Evergage.cashDom(event.target).closest(".save").length > 0) {
                            const step = Evergage.cashDom("#progressbar .active").length;
                            if (step > 0) {
                                const email = Evergage.cashDom("#form-application-email").val();
                                let actionEvent = {
                                    user: {
                                        attributes: {LifecycleState: "Mortgage Application Open"}
                                    },
                                    action: "Get Pre-Approved Form Step " + step + " Save For Later"
                                };
                                if (email) {
                                    actionEvent.user.id = email;
                                }
                                Evergage.sendEvent(actionEvent);
                            }
                        }
                    })
                ]
            }
        ]
    };

    Evergage.initSitemap(config);
});
```