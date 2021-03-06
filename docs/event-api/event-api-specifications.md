---
path: '/event-api/event-api-specifications'
title: 'Event API Requests'
tags: ['api', 'event']

---
The Interaction Studio Event API uses standard HTTP methods and returns JSON-formatted responses to requests generated by the Sitemap and other Interaction Studio event sources. The following sections describe the various Event API endpoints that enable you to interact with the Interaction Studio platform.

---
## Send Event Data

Send event data to the Interaction Studio dataset specified in the request.

### Method and Endpoint

`POST` `/api2/event/<dataset>`

### Request URL

`https://<accountName>.<server>.evergage.com/api2/event/<dataset>`

### URL Parameters

| URL Parameter | Required? | Description |
|---|---|---|
| `accountName` | Yes | Your Interaction Studio account name. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `https://demo.us-1.everage.com/`, then your account name is `demo`. |
| `server` | Yes | Your Interaction Studio server identifier. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `https://demo.us-1.everage.com/`, then your server identifier is `us-1`. |
| `dataset`| Yes | The name/identifier of the Interaction Studio dataset you are sending data to. |

### Supported Content Types

* `application/json`
* `application/x-www-form-urlencoded`

### Examples

Consider the following as sample event data that you would like to send to Interaction Studio. 

```json
{
    "action": "hello world",
    "user": {
        "id": "testuser"
    }
}
```

You can send this event data to Interaction Studio as a JSON payload by [minifying](https://www.npmjs.com/package/json-minify) the event data to remove whitespaces, including it in the body of your API request, and setting the `Content-Type` to `application/json`, as shown in the following example request.

```curl
curl -X POST \
     -H 'Content-Type: application/json' \
     -d '{"action":"hello world","user":{"id":"testuser"}}' \
     "https://<accountName>.<server>.everage.com/api2/event/<dataset>"
```
<div class='alert-blue'>

**Note**: You should not make `POST` requests with `Content-Type: application/json` from an environment that checks for Cross-Origin Resource Sharing (CORS), such as a web browser.
</div>

When using `Content-Type: application/x-www-form-urlencoded`, the event data must be minified and then base64-encoded. Minifying the example event data JSON above gives you the following single-line JSON string.

```json
{"action":"hello world","user":{"id":"testuser"}}
```

Base64-encoding the above JSON string gives you the value `eyJhY3Rpb24iOiJoZWxsbyB3b3JsZCIsInVzZXIiOnsiaWQiOiJ0ZXN0dXNlciJ9fQ==`. You can then pass this value via the `event` key, as shown in the following example request.

```curl
curl -X POST \
     -H 'Content-Type: application/x-www-form-urlencoded' \
     -d 'event=eyJhY3Rpb24iOiJoZWxsbyB3b3JsZCIsInVzZXIiOnsiaWQiOiJ0ZXN0dXNlciJ9fQ==' \
     "https://<accountName>.<server>.evergage.com/api2/event/<dataset>"
```

---

## Send Event Data Using a GET Request

Send event data to the Interaction Studio dataset specified in the request.

### Method and Endpoint

`GET` `/api2/event/<dataset>`

The event data you send in `GET` requests must be minified, base64-encoded, and passed via the `event` query parameter in the request URL.

### Request URL

`https://<accountName>.<server>.evergage.com/api2/event/<dataset>?event=<base64-encoded-minified-event>`

### URL Parameters

| URL Parameter | Required? | Description |
|---|---|---|
| `accountName` | Yes | Your Interaction Studio account name. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `demo.us-1.everage.com`, then your account name is `demo`. |
| `server` | Yes | Your Interaction Studio server identifier. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `demo.us-1.everage.com`, then your server identifier is `us-1`. |
| `dataset`| Yes | The name/identifier of the Interaction Studio dataset you are sending data to. |
| `base64-encoded-minified-event` | Yes | The minified and base64-encoded event data string |

### Example

Consider the following as sample event data that you would like to send to Interaction Studio.

```json
{
    "action": "hello world",
    "user": {
        "id": "testuser"
    }
}
```

You can pass this event by minifying the event JSON data and base64-encoding the resulting JSON string in the `event` parameter in the request URL and send the `GET` request, as shown in the following example.

```curl
curl -X GET 'https://<accountName>.<server>.evergage.com/api2/event/<dataset>/event=eyJhY3Rpb24iOiJoZWxsbyB3b3JsZCIsInVzZXIiOnsiaWQiOiJ0ZXN0dXNlciJ9fQ=='
```

You can also send the `GET` request via a web browser by accessing the request URL directly in your browser.

<div class='alert-blue'> 

**Note:** You can use `GET` requests as an alternative to using `POST` requests for sending event data to your Interaction Studio datasets, if required.
</div>

---

## Send Event Data via Trusted Channel

The Interaction Studio Event API provides a dedicated endpoint sending events that use _trusted_ `Server` channels. The API call must also be authenticated with a valid API token that is permitted to send events. The API token must be provided via the `Authorization` header according to the HTTP basic authentication scheme. For more information about creating and configuring an API token in Interaction Studio, see the [API Tokens](https://doc.evergage.com/display/EKB/API+Tokens) documentation.
 `Content-Type: application/json`

<div class='alert-blue'> 

**Note:**
- For datasets created after January 28, 2021, the campaign targeting rule is set to `channel: Server` by default. To return server-side campaigns using the Event API, an event be sent via a trusted server channel. For more information on the implications of using or removing the `channel: Server` rule, refer to the [Server-Side Templates](https://developer.evergage.com/campaign-development/server-side-campaigns-and-templates#determine-campaign-identity--authentication-requirements) documentation. 
- To add authentication to server-side campaigns for datasets created prior to January 28, 2021, please reach out to our [Support](https://doc.evergage.com/display/EKB/Open+Support+Ticket) team.
</div>

### Method and Endpoint

`POST` `/api2/authevent/<dataset>`

### Request URL

`https://<accountName>.<server>.evergage.com/api2/authevent/<dataset>`

### URL Parameters

| URL Parameter | Required? | Description |
|---|---|---|
| `accountName` | Yes | Your Interaction Studio account name. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `demo.us-1.everage.com`, then your account name is `demo`. |
| `server` | Yes | Your Interaction Studio server identifier. You can retrieve this by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `demo.us-1.everage.com`, then your server identifier is `us-1`. |
| `dataset`| Yes | The name/identifier of the Interaction Studio dataset you are sending data to. |

### Supported Content Types

`application/json`

### Authorization

`Authorization: Basic <base64-encoded-API-token>`

### Example

Consider the following as sample event data that you would like to send to Interaction Studio.

```json
{
    "action": "hello world",
    "source": {
        "channel": "Server"
    },
    "user": {
        "id": "testuser"
    }
}
```

You can send this event data via the trusted channel via a `POST` request to the dedicated endpoint, as shown in the following example request. Ensure that you minify the event JSON data into a single line by removing all whitespaces before sending it via the `POST` request.

```curl
curl -X POST \
     -H 'Content-Type: application/json' \
     -H 'Authentication: Basic <base64-encoded-API-token>' \
     -d '{"action":"hello world","source":{"channel":"Server"},"user":{"id":"testuser"}}' \
     "https://<accountName>.<server>.evergage.com/api2/authevent/<dataset>"
```

---
## Event Options

```javascript
{
    action: string,
    itemAction: string,
    debug: {
        explanations: boolean,
        testMessages: string
    },
    flags: {
       noCampaigns: boolean,
       doNotTrack: boolean,
       pageView: boolean,
       nonInteractive: boolean
    },
    source: {
        channel: string,
        pageType: string,
        url: string,
        urlReferrer: string,
        locale: string,
        contentZones: string[],
        time: number
    },
    user: {
        id: string,
        attributes: {
            [key: string]: string | number | boolean
        }
    },
    account: {
        id: string,
        domain: string,
        attributes: {
            [key: string]: string | number | boolean
        },
    },
    catalog: {
        <ItemType>: {
            [key: string]: string | number | boolean | object
        }
    },
    order: {
        <ItemType>: {
            orderId: string,
            totalValue: number,
            currency: string
            lineItems: LineItem[]
        }
    },
    cart: {
        singleLine: {
            <ItemType>: LineItem
        },
        complete: {
            <ItemType>: LineItem[]
        }
    },
    campaignStats: CampaignStat[],
    performance: {
        sdkLoadTime: number,
        pageLoadTime: number,
        sdkParseTime: number,
        networkTime: number,
        sdkDnsTime: number,
        eventDnsTime: number,
        downloadTime: number
    }
}
```

---

`action: string`

Name of the event. This is used for segmentation, targeting and reporting.

`itemAction: string`

Defines the action taken on a catalog item.

---

## The debug object

_Fields to help investigate issues that could arise when developing campaigns_

`explanations: boolean`

If true, then return additional information in the response about why campaigns did or did not render. Requires authentication.

`testMessages: string`

A comma seperated list of campaign experience IDs to force to return in the event, ignoring rules that would otherwise prevent the campaign from returning.

Alternatively, the string value 'true' can be used to return all campaigns in testing mode but all rules will be respected.

---

## The flags object

_Flags that alter default event processing. By default, all flags are `false` if not present on the event._

`noCampaigns: boolean`

If true, do not return campaigns in the response.

`pageView: boolean`

If true, indicates that the event was triggered from a page load.

`nonInteractive: boolean`

If true, a visit will not be created (or updated) for the given user in the event. Additionally, no visit referrer nor originating referrer will be created for the user.

`doNotTrack: boolean`

If true, do not process the event one it is sent to the server.

---

## The source object

_Describes where the event is coming from_

`channel: string`

The originating source of the event (e.g. Web, MobileApp, CallCenter).

`application: string`

The originating application level source of the event (e.g. ReactApp, 3rd Party, ReactNative)

`pageType: string`

The type of page from which you are sending the event (e.g. PDP, Blog, Pricing).

`url: string`

The url of the page from which you are sending the event.

`urlReferrer: string`

The previous url visited by the user.

`locale: string`

The locale of the current page, as defined by ISO 639 alpha-2 language codes and ISO 3166 alpha-2 country codes (e.g. `en_US`, `de_DE`).

`contentZones: string[]`

An array of content zones on the current page.

`configVersion: number`

Version number of the configuration for the SDK.

`time: number`

The date and time of the event, in milliseconds elapsed since the UNIX epoch. This field is optional and will be autopopulated with the current time if null.

---

## The user object

_Describes the user associated with the event_

`id: string`

The ID of a known user.

`attributes: { [key: string]: string | number | boolean } `

Key-value pairs that are stored as metadata on the user. These attributes must be defined in the platform.

---

## The account object

_Describes the account associated with the event_

`id: string`

The ID of an account.

`encryptedId: string`

The encrypted ID returned from an event that contains the ID field above. Encrypted IDs are returned in the response to events which provide a user.id.

`attributes: { [key: string]: string | number | boolean }`

Key-value pairs that are stored as metadata on the account. These attributes must be defined in the platform.

---

## The catalog object

_Item data associated with the given itemAction_

Used for actions pertaining to items in the catalog. For instance, provides the item being view when sending the item action "View Item"

```javascript
{
    ...
    catalog: {
        <ItemType>: {
            [key: string]: string | number | boolean | object
        }
    },
    ...
}
```

See the articles in the [Sitemap](/web-integration/sitemap) section of this site for more information.

---

## The order object

_Order data associated with the given itemAction_

Used for Purchase actions.

```javascript
{
    ...
    order: {
        <ItemType>: {
            orderId: string,
            totalValue: number,
            currency: string
            lineItems: LineItem[]
        }
    },
    ...
}
```
###### Line Item
```javascript
    {
        _id: string,
        price: number,
        quantity: number
    }
```


`orderId: string`

ID of the purchased order.

`currency: string`

Optional currency code of purchase. Will default to the dataset's configured currency if null.

`totalValue: number`

Optional total value for purchase. Will default to sum of line items quantity * price.

`lineItems: LineItem[]`

List of purchased line items.

See the articles in the [Sitemap](/web-integration/sitemap) section of this site for more information.

---

## The cart object

_Cart data associated with the given itemAction_
```javascript
{
    ...
    cart: {
        singleLine: {
            <ItemType>: LineItem
        },
        complete: {
            <ItemType>: LineItem[]
        }
    }
    ...
}
```

`singleLine: LineItem`

Used for Add To Cart and Update Cart actions. Add To Cart will add the quantity to any existing matching items. Update Cart will overwrite the quantity of any existing matching items.

See the articles in the [Sitemap](/web-integration/sitemap) section of this site for more information.

`complete: LineItem[]`

Used to update the state of the cart in Interaction Studio, setting it's content to the provided line items. Previous cart contents will be replaced.

---

## The campaignStats object

_Campaign stats to be tracked with the associated event. For sending campaign stats on the web, see [Campaign Stats Tracking](/campaign-development/web-templates/web-campaign-stats)._

```javascript
{
    ...
    {
        campaignStats: CampaignStat[]
    }
    ...
}
```

##### CampaignStat

```javascript
{
    experienceId: string,
    stat: "Impression" | "Click" | "Dismissal",
    control: boolean,
    catalog: {
        <ItemType>: string[]
    }
}
```

`experienceId: string`

The experience ID of the campaign on which the given stat is being tracked

`stat: "Impression" | "Click" | "Dismissal"`

The type of stat that is being tracked.

`control: boolean`

If true, the stat is tracked for the control group of the given experience

`catalog: { <ItemType>: string[] }`

A mapping of catalog item types to a list of corresponding item IDs on which to attribute the given stat.

```javascript
...
catalog: {
    Product: ["product1", "product2"]
}
...
```

---

## The performance object

_Measurements of loading, parsing and network performance_

`sdkLoadTime: number`

(Web SDK) Time, in milliseconds, for the network to load the web SDK.

`pageLoadTime: number`

(Web SDK) Time, in milliseconds, for the DOM to load.

`sdkParseTime: number`

(Web SDK) Time, in milliseconds, for the beacon to be parsed during page load.

`networkTime: number`

(Web SDK) Time, in milliseconds, for the previous request to return.

`sdkDnsTime: number`

(Web SDK) Time, in milliseconds, to perform resolution of SDK's cdn domain.

`eventDnsTime: number`

(Web SDK) Time, in milliseconds, to resolve account specific domain.

`domLoadTime: number`

(Web SDK) Time, in milliseconds, for the DOM to finish loading.
