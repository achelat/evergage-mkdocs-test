---
path: '/interface-config/import-export'
title: 'Importing And Exporting Configurables'
tags: ['config', 'gears', 'template']
---

Gear Components and Templates can import configurable properties from other Gear Components. This allows the shared use of configuration objects that can be passed to exported methods, allowing gears and templates to interact with gears.

_It is important to note that while templates export their main CampaignTemplate class, no classes from within a template can be accessed by a gear or another template._

Export and imports are handled the same as TypeScript modules, utilizing destructured assignment to declare the class intended to be imported, and the gear id as the module.

MyGearComponent.ts _contained within 'mygear' gear._
```ts
export class MyConfig {
    firstProperty: string;
    secondProperty: number;
}
```

MyTemplate.ts
```ts
import { MyConfig } from "mygear";

export class MyTemplate implements CampaignTemplate {
    myConfig: MyConfig;
}
```