---
path: '/gears/components/contextual-rule'
title: 'Contextual Rule Component'
tags: ['gears', 'components', 'contextual rule']
---


### Overview
A Contextual Rule component is used to configure a custom rule that may apply in a particular context, dictated by the
Gear component. For example, you can create a rule that will filter out promotions in a Contextual Bandit
campaign based on whether a user has an attribute that matches the eligible promotion's dimension. These 
filtering rules can be checked when applied to any campaign, bandit arm, or segment. If a rule is not applicable
for a particular environment, the filter won't apply.


#### Methods and Implementation

`Generic` - What is the type of data that the Gear component is using to identify whether this filter should apply? Examples include, `string` if trying to determine a particular dimension's value or `number` if the rule is based on a user's LTV. This should go into the `<>` brackets when declaring a class which implements `ContextualRule`.

`contextValue()` - A method which returns the value the rule needs to calculate whether or not it matches. Return type must correspond to the type declared as the generic.

`matches()` - A method returning a boolean which is the actual logic for the rule.

`metadata()` - A method returning RuleMetadata, an object containing information about the rule itself, including the name category it applies to, and its compatible environments. Rule will not be applied in non-compatible environments.

`description()` - A method returning a string which summarizes the rule (for the UI).

`abbreviatedDescription()` - A method returning a string corresponding to a very brief (one or two word) summary of the rule to appear in the template editor UI.

`logo()` - An optional method to return a font awesome icon which will appear in the UI.

``

#### Example
`WeekdayPreference.ts`
```typescript
export class WeekdayPreference implements ContextualRule<string> {

    readonly doc: string = "'This' matches their weekend/weekday preference.";

    metadata(): RuleMetadata {
        return {
            name: 'Weekday Preference',
            category: 'Hotel',
            compatibleEnvironments: ["Promotion", "Campaign", "Segment"]
        }
    }

    description(): string {
        return "'This' matches their weekend/weekday preference.";
    }

    abbreviatedDescription(): string {
        return "Weekday";
    }

    logo(): string {
        return "fa-hotel";
    }

    contextValue(context: ContextualRuleContext): string {
        const stayTypeAttr = context.user.attributes["StayType"];
        if (!context.item || !context.item.dimensions || !context.item.dimensions["ItemClass"] || !stayTypeAttr) {
            return null;
        }
        let stayType: string = stayTypeAttr.value as string;
        return context.item.dimensions["ItemClass"].indexOf(stayType) >= 0 ? stayType : null;
    }

    matches(context: ContextualRuleContext): boolean {
        const stayTypeAttr = context.user.attributes["StayType"];
        if (!context.item || !context.item.dimensions || !context.item.dimensions["ItemClass"] || !stayTypeAttr) {
            return true;
        }
        return this.contextValue(context) != null;
    }
}
```
