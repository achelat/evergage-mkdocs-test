---
path: '/gears/components/lifecycle'
title: 'Lifecycle Component'
tags: ['gears', 'component', 'lifecycle']
---

Lifecycle components represent the installation and use lifecycle of a Gear within the Interaction Studio Platform. 
Gear Lifecycles can have install methods that interact with the configuration interfaces in order to 
setup the Interaction Studio Platform for use by the Gear. This can include things like defining new catalog dimensions
or setting up new user profile attributes.

### Configuration
Lifecycle components can have Gear configs that present configuration interfaces to the end users. Simply adding 
config style properties to the component will render that configuration interface according using the
[Gear Configuration](/interface-config) system.



## Example

`ExampleLifecycle.ts`
```typescript
export class SportsGearLifecycle implements GearLifecycle {
    
    title : string =  "Favorite Sports";
    
    install(context : GearLifecycleContext) {
        // Install a new dimension to track "Sports"
        context.services.systemConfiguration.requestCatalogConfig(
            { name : "Sport", label : "Sport", description : "Games people play", enabled: true}
        )
        
        // Install a user attribute for their favorite sport
        context.services.systemConfiguration.requestUserAttribute(
            {id : "favoriteSport", label : "Favorite Sport", type: "String"}
        )

        context.services.systemConfiguration.submitPendingRequests();
    }
    
    validate() {
        let validator = new Validator<this>(this);
        return validator.errors;
    }
}
```
