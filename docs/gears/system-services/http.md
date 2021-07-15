---
path: '/gears/system-services/http'
title: 'HTTP Service'
tags: ['gears', 'services', 'http']
---

The HTTP service facilitates making outbound HTTP network requests from a Gear. All standard HTTP methods (such as GET, POST, PUT) are supported. All requests made from this service are synchronous and will return a [ClientResponse](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/clientresponse.html) upon completion. For documentation on all available methods see the [HTTP API Typedoc](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/http.html).

#### Example
```typescript
let response: ClientResponse = context.services.http.get('https://evergage.com');
console.log(response.body.asJson());

```

## Authentication
The HTTP service can be used in conjunction with any of the three authentication Gear components: [OAuth](/gears/components/oauth), HTTP Basic Auth and Custom Auth. Once the applicable authentication component is configured, it can be applied to the [ClientRequest](http://evergage-gears-docs.s3-website-us-east-1.amazonaws.com/core/interfaces/clientrequest.html) which is passed as the second parameter to any of the HTTP service methods.

#### Example
```typescript
let request: ClientRequest = { authenticationType: 'OAuth' }; // Replace 'OAuth' with 'HttpBasic' or 'Custom'
let response: ClientResponse = context.services.http.get('https://evergage.com', request);
```


## Get Request
#### Example
```typescript
console.info("Get Request");
let url = "https://postman-echo.com/get?foo1=bar1&foo2=bar2"
let response = context.services.http.get(url, {});
response.close();
```

## Post Request
#### Example
```typescript
console.info("Post Request");
url = "https://postman-echo.com/post";
response = context.services.http.post(url, { body : { testKey : "testValue"}});
response.close();
```


## Put Request
#### Example
```typescript

console.info("Put Request");
url = "https://postman-echo.com/put";
response = context.services.http.put(url, { body : { testKey : "testValue"}});
response.close();
```


## GZIP Responses
#### Example
```typescript
console.info("GZip Response");
url = "https://postman-echo.com/gzip";
response = context.services.http.get(url, { body : { testKey : "testValue"}});
response.close();
```


## Basic Auth Request
#### Example
```typescript
console.info("Basic Auth Request");
url = "https://postman-echo.com/basic-auth";
let authorization = `Basic ${btoa("postman:password")}`;
response = context.services.http.get(url, { headers : { Authorization : authorization }, body : { testKey : "testValue"}});
response.close();
```


## Retrieve IP Address
#### Example
```typescript
console.info("Get IP Address");
url = "https://postman-echo.com/ip";
response = context.services.http.get(url, {});
console.log(`Your IP Address is ${response.body.asJson()['ip']}`)
response.close();


```







