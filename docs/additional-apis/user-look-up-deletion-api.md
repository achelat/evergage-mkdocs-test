---
path: '/additional-apis/user-look-up-deletion-api'
title: 'User Look-Up & Deletion API'
tags: ['api', 'gdpr', 'ccpa']
---

The Interaction Studio User Look-Up & Deletion API helps you retrieve and delete user data on demand so that your business stays in compliance with the European Union's General Data Protection Regulation (GDPR) and the California Consumer Privacy Act (CCPA). In addition to using the [Interaction Studio UI](https://doc.evergage.com/display/EKB/Service+GDPR+and+CCPA+Data+Edit+or+Deletion+Requests), you can use this API to service *Right of Access* or *Right to be Forgotten* requests by either retrieving or permanently deleting all data and information captured about a user by your business. Interaction Studio processes deletion requests as soon as the requests are received.

The User Look-Up & Deletion API uses API tokens to authenticate all data change requests. An Account Administrator can generate API tokens from the Interaction Studio UI. For more information, read the article about [API Tokens](https://doc.evergage.com/display/EKB/API+Tokens) on the Interaction Studio Knowledge Base.

The following sections describe the various operations you can carry out using the API, and the associated endpoints.

---

## Export User by userId or anonId

Find and export a user in an Interaction Studio dataset by `userId` (if named) or `anonId`.

### Endpoint

```
GET /api/dataset/{dataset}/user/{userId}
```

### Response Content Type

`application/json`

### Response Example

```json
{
	userSummary: {_id: "622bb783f91f82c0",…},
	anonAliases: ["622bb783f91f82c0"],
	anonMergeTimes: [1541189046207],
	anonymous: true,
	attributes: [{name: "oneSignalClickthroughRate", value: 0, updated: 1541190782448}]
	displayName: null,
	engagement: {score: 121, date: 1541116800000},
	firstActivity: 1480014079580,
	lastActivity: 1541190778395,
	location: {deviceProvided: false, longlat: [-92.3241, 35.2193], timeZone: "America/Chicago",…},
	namedUserId: "622bb783f91f82c0",
	orderHistory: [,…],
	originatingReferrer: {medium: "SEARCH", source: "Google", domain: "google.com", 	subdomainReversed: "com.google.www",…},
	pageLocale: null,
	segmentMembership: [{id: "1PPOO", joined: 1536114160926}, {id: "2Hh5C", joined: 1527014815907},…],
	updated: 1541190838077,
	visitHistory: [{start: 1480014079580, lastEventTime: 1480014699812, visitIndex: 1,
	_id: "622bb783f91f82c0",
...
}
```

---

## Remove a User from Interaction Studio

Find and delete user(s) from an Interaction Studio dataset with user identifiers submitted via a CSV file.

### Endpoint

```
POST /api/dataset/{dataset}/users/delete
```

### Request Content Type

`multipart/form-data`

### Request Example

```
name
tom@example.com
dick@example.com
harry@example.com
```

---

## Example - Deleting User Data with an API

The following section describes the steps to delete user(s), and their associated data with the Interaction Studio API, along with example Bash and curl commands. You can modify and use these example commands to send in your own deletion requests via the User Look-Up & Deletion API.

1. Export your Interaction Studio account name and server identifier using Bash, as shown in the following example.

      ```bash
      export ACCOUNT='<account>.<server>' # your account name and server
      ```
   **Note:** You can retrieve your Interaction Studio account name and server by accessing **Gears** from the Interaction Studio UI and reviewing the URL. For example, if your Gears URL is `demo.us-1.everage.com`, then your account name is `demo` and server identifier is `us-1.`

1. Export the `DATASET` your user(s) belong to, as shown in the following example.

      ```bash
      export DATASET='datasetName' # the name of your dataset
      ```

1. Export the API token to be used with the API request, as shown in the following example.

      ```bash
      export HTTP_BASIC_AUTH_STRING='<encoded-api-credential>' # Where <encoded-api-credential> is a strict Base64 encoding of your <api-key-id>:<api-secret-key> 
      ```
   **Note:** Your API token must be a strict Base64 encoding of your **API Key ID** and its **API Secret Key**. You can use an existing API token if it has the required permissions or generate a new one from the Interaction Studio UI by navigating to **Security** > **API Tokens** > **Create Token**. For more information, see [API Tokens](https://doc.evergage.com/display/EKB/API+Tokens#APITokens-CreateCreateanAPIToken).

1. Export the CSV file to be submitted with your API request as a variable, as shown in the following example.

      ```bash
      export CSV_FILE='users.csv'
      ```

1. Write the identifiers of the user(s) you'd like to delete to the CSV file, as shown in the following example.

      ```bash
      echo -e "name\ntom@example.com\ndick@example.com\nharry@example.com" > ${CSV_FILE}
      ```

1. Construct and send a curl request to delete users using the variables you declared in the previous steps, as shown in the following example.

      ```curl
      curl --form "file=@${CSV_FILE}" -H "Authorization: Basic ${HTTP_BASIC_AUTH_STRING}" "https://${ACCOUNT}.evergage.com/api/dataset/${DATASET}/users/delete"
      Deleted 3 users (3 requested)
      ```

1. Confirm whether your deletion request was processed by sending the same curl request, as shown below.

      ```curl
      curl --form "file=@${CSV_FILE}" -H "Authorization: Basic ${HTTP_BASIC_AUTH_STRING}" "https://${ACCOUNT}.evergage.com/api/dataset/${DATASET}/users/delete"
      Deleted 0 users (3 requested)
      ```
