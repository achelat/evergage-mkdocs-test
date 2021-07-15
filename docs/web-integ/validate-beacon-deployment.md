---
path: '/web-integration/sitemap/validate-beacon-deployment'
title: 'Validate JavaScript Beacon Deployment'
tags: ['site', 'map', 'sitewide', 'javascript', 'client', 'websdk']
---

Interaction Studio supports web channel campaigns by integrating with websites using a [JavaScript web beacon](/web-integration#web-sdk-javascript-beacon) that provides access to the Interaction Studio [Web SDK](/web-integration#web-sdk). Site pages that do not contain the beacon will have no behavioral tracking or personalization capabilities. Use the following steps to validate that the [JavaScript beacon](/web-integration) is on the desired website pages.  

---
**Note:** A configured [Sitemap](/web-integration/sitemap) is **not** required to validate deployment of the JavaScript beacon.

---

## Beacon Deployment Validation Steps

1. Check the integration type intended for your Interaction Studio web integration: See [Plan a Web Integration](/web-integration/web-integration-planning) on this site and [Synchronous vs Asynchronous Integration](https://doc.evergage.com/display/EKB/Synchronous+vs+Asynchronous+Integration) on the Interaction Studio business user knowledge base.
2. Confirm that the JavaScript beacon script tag is present in the HTML of the desired website pages. Example script tags:
    - Synchronous
    ```html
       <script type="text/javascript" src="//cdn.evgnet.com/beacon/myaccount/mydataset/scripts/evergage.min.js"></script>
    ```
    - Asynchronous
    ```html
        <script type="text/javascript">
        var _aaq = window._aaq || (window._aaq = []);

        (function(){
            var d = document, g = d.createElement('script'), s = d.getElementsByTagName('script')[0];
                g.type = 'text/javascript'; g.async = true;
                g.src = document.location.protocol + '//cdn.evgnet.com/beacon/myaccount/mydataset/scripts/evergage.min.js';
                s.parentNode.insertBefore(g, s);
        })();
        </script>
    ```

3. Open the Chrome DevTools **Console** panel and ensure the `window.Evergage` object is defined.