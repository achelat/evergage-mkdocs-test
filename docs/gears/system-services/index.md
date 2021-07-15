---
path: '/gears/system-services'
title: 'System Services'
tags: ['gears', 'services']
---
System Services are exposed to the Gears environment and provide access to the fundamental features of the Interaction Studio platform. The services are made accessible through the Gear component's context. The available services are determined by the Gear component. System Services are differentiated from [Components](/gears/components) in that Gears use System Services to call into the Interaction Studio system, while a component is called by the Interaction Studio system at a time determined by the component type.

#### Example
```typescript
export class SystemServiceExample implements EntityJob<User> {

    writer: FileWriter;

    begin(context: EntityJobContext) {
        this.writer = context.services.filesystem.writeFile("/users.txt"); // the filesystem service is accessed via the context
        this.writer.writeLine("user ids")
    }

    transform(context: EntityJobContext, user: User) {
        this.writer.writeLine(user.id);
    }
    
}
```


