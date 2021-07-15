---
path: '/web-integration/sitemap/site-map-user-account-attributes'
title: 'Capturing User and Account Attributes'
tags: ['site', 'map', 'sitewide', 'account', 'user', 'attribute', 'attributes']
---

User and account attributes are captured using an `onActionEvent` to add user data to the event before it is sent to Interaction Studio. The full structure of the `user` object is specified in the [Event API](/event-api) section of this site.

## onActionEvent in the global configuration
```js
global: {
  onActionEvent: (actionEvent) => {
    const email = Evergage.cashDom("header.logged-in-user-email").text().trim();
    if (email) {
        actionEvent.user = actionEvent.user || {};
        actionEvent.user.attributes = actionEvent.user.attributes || {};
        actionEvent.user.attributes.emailAddress = email;
    };
    return actionEvent;
  }
}
```

## onActionEvent in the pageType configuration
```js
pageTypes: [
    {
        name: "search",
        isMatch: () => /\/search/.test(window.location.pathname),
        onActionEvent: (actionEvent) => {
            const searchTerm = Evergage.cashDom(".content.searchTerm").text().trim();
            if (searchTerm) {
                actionEvent.user = actionEvent.user || {};
                actionEvent.user.attributes = actionEvent.user.attributes || {};
                actionEvent.user.attributes.lastSearchTerm = searchTerm;
            };
            return actionEvent;
        }
    },
],
```

# Setting user and account attributes using event listeners
The code below is in the global configuration. It listens on every page for a form located in the footer to be submitted. It then sends an event containing the user's `id` and captures the user's email address, which can then be configured for use as an [identity](/data-model/identity). If you would like to utilize captured data as an `id`, please refer to the [Event API](/event-api) documentation.

```js
global: {
    listeners: [
        Evergage.listener("submit", "form.footer", () => {
            const email = Evergage.cashDom("form.footer input.email").val();
            Evergage.sendEvent({
                action: "Form Submission | Footer",
                user: {
                    attributes: {
                        emailAddress: email,
                    }
                }
            });
        })
    ]
},
```

This example shows a listener in the page type configuration. You can see that the structure of the event is the same as event sent in the global listener in the previous example.

```js
pageTypes: [
    {
        name: "preferences",
        isMatch: () => /\/preferences/.test(window.location.pathname),
        listeners: [
            Evergage.listener("submit", "form.user-prefs", () => {
                const email = Evergage.cashDom("form.user-prefs input.email").val();
                Evergage.sendEvent({
                    action: "Form Submission | Preferences",
                    user: {
                        attributes: {
                            emailAddress: email,
                        }
                    }
                });
            })
        ]
    },
],
```

Capturing `account` attributes works the same way as capturing `user` attributes. The full structure of the `account` object is specified in the [Event API](/event-api) section of this site.

```js
global: {
    listeners: [
        Evergage.listener("submit", "form.survey", () => {
            const industry = Evergage.cashDom("form.survey input.industry").val();
            const employeeSize = Evergage.cashDom("form.survey input.employeeSize").val();
            Evergage.sendEvent({
                action: "Form Submission",
                account: {
                    attributes: {
                        industry: industry,
                        employeeSize: employeeSize
                    }
                }
            });
        })
    ]
},
```
