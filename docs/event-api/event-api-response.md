---
path: '/event-api/event-api-response'
title: 'Event API Responses'
tags: ['api', 'event']

---

Requests sent to the Event API return JSON that contains information about any campaigns that will be returned. The shape of the response from the Event API is outlined below.

```javascript
{
    "campaignResponses": CampaignResponse[],
    "compiledCampaignTemplates": CompiledCampaignTemplate[],
    "campaignExplanations": CampaignExplanation[],
    "errorCode": number,
    "resolvedUserId": string,
    "persistedUserId": object,
    "eventDetails": eventDetails
}
```

`campaignResponses: CampaignResponse[]`

An array of all campaigns returned in the event. See [CampaignResponse Object](/event-api/event-api-response#the-campaignresponse-object) below.

`compiledCampaingTemplates: CompiledCampaignTemplate[]`

An array of all campaign templates returned in the event. See [CampaignExplanations Object](/event-api/event-api-response#the-compiledcampaigntemplates-object) below.

`errorCode: number`

The error code. Possible values are:

|errorCode	|Description	|
|---	|---	|
|0	|Success	|
|1	|Missing Encrypted UserId Parameter	|
|2	|Invalid Encrypted UserId Parameter	|
|3	|Busy	|
|4	|Anonymous ID Correction	|
|5	|Invalid Event	|

`resolvedUserId: string`

The primaryId of the resolved User. This is only returned if the _Pipeline Identity Resolution_ feature is enabled for the tenant. See [User Identity Mapping](/web-integration/user-identity-mapping) for more information.

`persistedUserId: object`

An object containing the encrypted user id of the user. This is only returned if the _Pipeline Identity Resolution_ feature is not enabled for the tenant and a primary user id is specified in the event. See [Identity](/web-integration/user-identity-mapping) for more information.

`eventDetails: eventDetails`

An object containing the event received by the server. This is only returned if `debug.explanations` was sent with the event and the event was authenticated.

### The CampaignResponse Object

_Data associated with the campaigns returned from an event_

```javascript
{
    "type": string,
    "campaignId": string,
    "campaignName": string,
    "campaignType": string,
    "experienceId": string,
    "experienceName": string,
    "experienceSourceCode": string,
    "state": string,
    "displayMode": string,
    "redirectUrl": null,
    "saveParameters": boolean,
    "hidePageBeforeRedirect": boolean,
    "campaignJavascriptContent": null,
    "javascriptContent": null,
    "userGroup": string,
    "googleAnalyticsConfig": null,
    "messages": [],
    "pageChanges": [],
    "templateNames": string[],
    "serverSideMessages": ServerSideMessage[],
    "payload": payload
}
```

`type: string`

The type of campaign returned. For Server-Side campaigns this will be "c". For Web campaigns this will be "ng".

`campaignId: string`

The id of the campaign returned.

`campaignName: string`

The name of the campaign returned.

`campaignType: string`

The type of the campaign returned. For Server-Side campaigns this will be "ServerSide". For Web campaigns this will be "Web".

`experienceId: string`

The id of the experience returned.

`experienceName: string`

The name of the experience returned.

`state: string`

The state of the campaign returned. This can be "Published", "Disabled" or "Testing" depending on the campaign state.

`displayMode: string`

Deprecated. This is only returned for Server-Side campaigns and will always be "Personalized".

`redirectUrl: null`

Deprecated. This is only returned for Server-Side campaigns.

`saveParameters: boolean`

Deprecated. This is only returned for Server-Side campaigns and will always be `false`.

`hidePageBeforeRedirect: boolean`

Deprecated. This is only returned for Server-Side campaigns and will always be `false`.

`campaignJavascriptContent: null`

Deprecated. This will always be `null`.

`javascriptContent: null`

Deprecated. This is only returned for Server-Side campaigns and will always be `null`.

`userGroup: string`

The userGroup of the campaign experience returned. This can be "Default" or "Control".

`googleAnalyticsConfig: null`

Deprecated. This is only returned for Server-Side campaigns and will always be `null`.

`messages: []`

Deprecated. This is only returned for Server-Side campaigns and will always be `[]`.

`pageChanges: []`

Deprecated. This is only returned for Server-Side campaigns and will always be `[]`.

`templateNames: string[]`

An array of the template names used in the experience. This is only returned for Web campaigns.

`serverSideMessages: ServerSideMessage[]`

An array of messages returned for a Server-Side campaign. This is only returned for Server-Side campaigns. The event **must** be sent with `source.channel` set to "Server" in order to return `serverSideMessages`. See [ServerSideMessage object](#the-serversidemessage-object) below.

`payload: payload`

The payload object returned by the campaign. See [Server Typescript](/campaign-development/web-templates/web-template-server-typescript)

### The ServerSideMessage object

_Data associated with serverSideMessages returned in a campaignResponse_

An array of ServerSideMessage objects is returned for Server-Side campaigns. This object contains information about the data returned for the server-side campaign message. The event **must** be sent with `source.channel` set to "Server" in order to return `serverSideMessages`. See [Server-Side Campaigns](https://doc.evergage.com/display/EKB/Server-Side+Campaigns) for more information on configuring Server-Side Campaigns.

```javascript
{
    "type": string,
    "id": string,
    "data": string,
    "targetName": string,
    "containsDynamicContent": boolean,
    "promotedItemKeys": PromotedItem[] | null,
    "campaignState": string,
    "dataMap": dataMap
}
```

`type: string`

The type of message returned. This will always be "evg_md" for a ServerSideMessage.

`id: string`

The id of the message returned.

`data: string`

Deprecated. This will always be "{}".

`targetName: string`

The name of the target for the ServerSideMessage.

`containsDynamicContent: boolean`

True if the message contains any dynamic content. False if the message does not contain dynamic content.

`promotedItemKeys: PromotedItem[] | null`

If the message contains promoted items, this is an array of promoted items. Otherwise this is null.  Each PromotedItem returned contains the type of the item returned and the _id of the item returned. Example:

```javascript
{
    type: "p",
    _id: "123"
}
```

`campaignState: string`

The state of the campaign returned. This can be "Published", "Disabled" or "Testing" depending on the campaign state.

`dataMap: dataMap`

An object containing the key value pairs defined in the campaign. Example:

```javascript
dataMap: {
    "myText": "helloWorld",
    "myNumber": "1",
    "myPromotedContent": [
        {
            "type": "p",
            "_id": "123"
        },
        {
            "type": "p",
            "_id": "456"
        }
    ]
}
```

For keys with type "Promoted Content", the full value of each item will be returned unless the "Item IDs only" option is enabled in the message settings. See [Server-Side Campaigns](https://doc.evergage.com/display/EKB/Server-Side+Campaigns#Server-SideCampaigns-ConfigurePromotedContent) for more information on configuring Server-Side campaigns and the returned dataMap object.

### The CompiledCampaignTemplates object

_Data associated with campaignTemplates returned in an event_

An event response returns an array of any compiledCampaignTemplates utilized in a campaignResponse. The compiledCampaignTemplates object contains information about the campaign templates.

```javascript
{
    "id": string,
    "name": string | null,
    "bundle" string
}
```

`id: string`

The id of the template.

`name: string | null`

The name of the template.

`bundle: string`

The stringified [client-side code](/campaign-development/web-templates) used to render the template.

### The CampaignExplanations Object

_Data associated with campaignExplanations returned in an event_

campaignExplanations will only be returned in an event if `debug.explanations` is true in the event payload.  It contains additional information about why campaigns did or did not return in the event response. It is only returned if the event request is authenticated.

```javascript
{
    "class": string,
    "campaignId": string,
    "campaignName": string,
    "experienceId": string,
    "experienceName": string,
    "messageId": string,
    "explanationMessage": string,
    "context": string
}
```

`class: string`

Information of the type of explanation returned.

`campaignId: string`

The id of the campaign the explanation is for.

`campaignName: string`

The name of the campaign the explanation is for.

`experienceId: string`

The id of the experience the explanation is for.

`experienceName: string`

The name of the experience the explanation is for.

`messageId: string`

The id of the message the explanation is for.

`explanationMessage: string`

A message containing the reason the campaign was not returned in the response.

`context: string`

A string containing the campaign name, campaign id, experience name, and experience id for the message not returned in the response.
