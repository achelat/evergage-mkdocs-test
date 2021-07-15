---
path: '/gears'
title: 'Gears'
tags: ['gears']
---

Interaction Studio Gears are an application extension platform used to add custom business functionality to the Interaction Studio platform. An Interaction Studio administrator at your company can enable and disable Gears in the Interaction Studio UI.
<div class='alert'>
Gear development is managed by Interaction Studio. Gear development requests can be filed as feature requests <a href='https://trailblazer.salesforce.com/ideaSearch?filter=Marketing+%3E+Interaction+Studio'>here</a> for review by IS Product team members.
</div>

### TypeScript

All Gears, template and Web SDK code is in TypeScript, a typed superset of JavaScript that compiles to plain JavaScript.

For further reading, see https://www.typescriptlang.org/docs/home.html

### Modules

Each TypeScript source file within a gear is an ES module, which allows the developer and consumer the benefits of encapsulation, namespacing, and flexibility to avoid naming conflicts by renaming/aliasing namespaces.

A module has its own scope and must explicitly `export` what should be accessible to others. Similarly, it must `import` from other modules as desired.

Typically you'll `export` a class or function you wish to be available for use outside of that file. Imports do not include file extensions like `.ts`. See examples below.

For further reading, see https://www.typescriptlang.org/docs/handbook/modules.html

#### Example Imports/Exports

Gear id `gear-one` file `Utils.ts`:

```typescript
export function timesTwo(num: number): num {
    return num * 2;
}
```

Another `gear-one` file `GearOne.ts` importing from its own `Utils` with a relative path of `./`:

```typescript
import { timesTwo } from './Utils';

export class GearOne {
    constructor(private _state: number = 1) {}
    
    doubleAndGet() : number {
        this._state = timesTwo(this._state);
        return this._state;
    }
}
```

A different gear, id `gear-two` file `GearTwo.ts` should import from absolute path `gear-one` (the gear id):

```typescript
import { GearOne } from 'gear-one';

export class GearTwo {
    private _g1 : GearOne;
    
    constructor() {
        this._g1 = new GearOne(2);
    }
    
    doStuff(): number {
        return this._g1.doubleAndGet();
    }
}
```


### Gear Component Types

| Type                                                        | Description                                                                                | Network | Filesystem | System Services |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------ | ------- | ---------- | --------------- |
| [Lifecycle](/gears/components/lifecycle)                   |                                                                                            | ✓       | ✓          | ✓               |
| Campaign Service                                            | Exposes services to Server Side Campaign Sources                                           | 𐄂       | 𐄂          | ✓               |
| Event Component                                             | Executes                                                                                   | 𐄂       | 𐄂          | ✓               |
| Event Transform                                             | Update events at the front of the pipeline                                                 | 𐄂       | 𐄂          | ✓               |
| Visit Start Listener                                        | Executes at the start of a visit                                                           | 𐄂       | 𐄂          | ✓               |
| Visit End Listener                                          | Executes at the end of a visit (typically 30 minutes after last activity)                  | 𐄂       | 𐄂          | ✓               |
| Order Listener                                              | Executes on a conversion event                                                             | 𐄂       | 𐄂          | ✓               |
| Deferred Task                                               | An asynchronous task that can be fired from pipeline gears to do expensive work            | ✓       | ✓          | ✓               |
| Beacon Resource                                             | Resources to be included with and shipped with the beacon                                  | 𐄂       | 𐄂          | ✓               |
| API Service                                                 | Expose a custom API on a custom endpoint                                                   | 𐄂       | 𐄂          | ✓               |
| Segment Rule                                                | A custom segment rule                                                                      | 𐄂       | 𐄂          | ✓               |
| Segment Export                                              | Provides segment membership deltas that can be exported to a file or an external service.  | ✓       | ✓          | ✓               |
| Pipeline Transform                                          | Update users in the pipeline                                                               | 𐄂       | 𐄂          | ✓               |
| Workflow Trigger                                            | Decides in the pipeline on whether to trigger a workflow                                   | 𐄂       | 𐄂          | ✓               |
| Workflow Action                                             | A custom action that can be executed as part of a workflow                                 | ✓       | 𐄂          | ✓               |
| Email Sender                                                | Custom sending of triggered and batch emails                                               | ✓       | 𐄂          | ✓               |
| Campaign Server Side Code                                   | The server-side portion of custom campaign code                                            | 𐄂       | 𐄂          | ✓               |
| [ETL Job](/data-ingestion)                                                     | Executes file loading and output                                                           | ✓       | ✓          | ✓               |
| User Updater                                                | Update user during nightly, e.g. custom scoring and targeting                              | 𐄂       | 𐄂          | ✓               |
| Identity Resolver                                           | Resolves identity conflicts and merges user profiles                                       | 𐄂       | 𐄂          | ✓               |
| Audit Notifier                                              | Audits important system configuration events                                               | ✓       | 𐄂          | ✓               |
| Data Sync Updater                                           | After visit, sync data to a data warehouse                                                 | ✓       | ✓          | ✓               |
| Client Side JS Bundle                                       | Add JavaScript to the beacon                                                               |         |            |                 |
| Custom Report                                               | A report with customized output based on selected metrics and data                         |         |            |                 |
| Batch Job                                                   | Fetches objects in iterative batches which can be staged and committed.                    | ✓       | 𐄂          | ✓               |
| Entity Job                                                  | Iterates over Interaction Studio Users, Accounts or Catalog Items to perform updates or export data. | ✓       | ✓          | ✓               |
| [OAuth](/gears/components/oauth)                           | Configures authentication with external services via OAuth.                                | 𐄂       | 𐄂          | 𐄂               |
| HTTP Basic Auth           | Configures authentication with external services via HTTP basic authentication.            | ✓       | 𐄂          | 𐄂               |
| Custom Auth              | Configures authentication with external services via a custom authentication scheme.       | ✓       | 𐄂          | 𐄂               |

### System Services

| System Services                                                    | Description                                                                    |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| Pipeline Catalog       | Access to the catalog in pipeline component types (real-time)                  |
| Catalog                         | Provides access to Items within the Interaction Studio Platform's catalog                |
| Defer                             | Defers tasks to be run at a later point in time                                |
| [Filesystem](/gears/system-services/filesystem)                   | Provides a virtualized filesystem                                              |
| [CSV](/gears/system-services/filesystem#csv)                      | Provides a virtualized filesystem for parsing CSV files                        |
| [XML](/gears/system-services/filesystem#xml)                      | Provides a virtualized filesystem for parsing XML files                        |
| S3 Filesystem             | Provides a virtualized filesystem backed by AWS S3                             |
| [HTTP](/gears/system-services/http)                               | Used for accessing HTTP resources                                              |
| Authorization             | Provides access to the authorization and authentication state of the gear to external systems. |
| Profile                         | Access to the Unified Customer Profile of a user within the Interaction Studio platform. |
| Recommendations         | Access to the Interaction Studio Platform's recommendation engine.                       |
| SmartTrends               |                                                                                |
| Campaign                      |                                                                                |
| Campaign Statistics |                                                                                |
| Decisions                     |                                                                                |
| Subscription               |                                                                                |

### <a name="Glossary"></a>Glossary

| Term           | Description                                                                                                                        |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Gear           | A bundled feature that extends the Interaction Studio platform. Gears may be offered by Interaction Studio or custom developed.                    |
| Gear Component | Each Component of a Gear provides source code that implements a Component Type.                                                    |
| Component Type | A specific integration hook into the Interaction Studio platform. Abstract class to be implemented by a Gear Component.                      |
| System Service | These services are exposed to the Gears environments and provide access to the fundamental features of the Interaction Studio platform.      |
| Gears Context  | Gears contexts are the environments exposed to the executing Gears code. The shape of these context depends on the component type. |
| Core Gears     | These gears deliver core features of the Interaction Studio platform. Core gears may be overridden and customized per environment.           |
| Private Gears  | These gears are specific to your environment and are not visible to other environments.                                            |
| Shared Gears   | Shared gears are available in the gears marketplace and can be installed, according to licensing agreements in each environment.   |
