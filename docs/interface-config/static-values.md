---
path: '/interface-config/static-values'
title: 'Static Values'
tags: ['config', 'gears', 'template']
---

The config system can also output static text meant to help a user utilize the interface. This can be documentation and explanation or even a dynamicly calculated phrase.

```ts
readonly staticText = "This text will appear in the output"
```

Using fat arrow functions, you can also make the output text dynamic.
```ts
name : string; 
readonly output = () => `Hello ${this.name}!`;
```

The readonly fat arrow functions are passed a [GearLifecycleContext](/gears/components/lifecycle) providing access to [System Services](/gears/system-services).
```ts
name : string; 
readonly output = (context: GearLifecycleContext) => {
    let response = context.services.http.get('https://somedomain.com/resource', {});
    `Hello ${response.body.asJson().name}!`
};
```