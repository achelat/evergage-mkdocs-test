---
path: '/interface-config/properties/complex'
title: 'Complex Object Property'
tags: ['config', 'gears', 'template']
---

#### Complex Types
Configurations can be more complicated groups of fields represented by another TypeScript class. Those classes can have multiple properties and use all the same features of the properties in the parent configuration class. 

```ts
export class ComplexType {
    firstProperty: string;
    secondProperty: number;
}

export class SimpleTemplate implements CampaignTemplate {
    myProperty: ComplexType;
}
```
![Complex](Complex.png) 

Complex Types can be shared between gears, and those defined within gears can be utilized by Templates. See [Imports & Exports](/interface-config/import-export) for more information.

_Related: [Select Property](/interface-config/properties/select), [Arrays](/interface-config/arrays), [Imports & Exports](/interface-config/import-export)_