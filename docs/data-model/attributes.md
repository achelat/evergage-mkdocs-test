---
path: '/data-model/attributes'
title: 'Attributes'
tags: ['Attributes', 'Data Model']
---

Attributes are custom-configured, type-safe fields that can store additional custom data at various levels in the platform. Attributes can be configured for users, accounts, orders, catalog items and dimensions.

### Supported Value Types

| Value Type | Description |
| ---- | ----------- |
| String | Text information |
| Integer | Whole numbers |
| Decimal | Floating point numbers |
| Date | Date and time, ie JavaScript `Date` |
| Boolean | `true` or `false` |
| Object | Arbitrary object/array of JSON plus Date support |
| null | Attribute present but no value |

### Details 

Attribute collections are keyed off the attribute name/id:
```ts
attributes: { [attributeName: string]: Attribute };
```

Each `Attribute` contains a `value` and optional `metadata` about the value:

```ts
interface Attribute {
    readonly value: AttributeValue;
    readonly metadata?: Readonly<ValueMetadata>;
}
```

A value can be of type `string`, `number`, `boolean`, `null`, `Date`, or a (potentially nested) collection containing them:


```ts
type AttributeValue = string | number | boolean | Date | null | AttributeValueObject | AttributeValueArray;

interface AttributeValueObject {
    readonly [index: string]: AttributeValue;
}

interface AttributeValueArray extends ReadonlyArray<AttributeValue> { }
```

The optional `metadata` can contain detailed information about the value:

```ts
/**
 * Source: https://github.com/usnistgov/NISTIR-8112/
 *
 * Glossary:
 *
 * Attribute Provider (AP) Manages and provides assertions of identity attributes to other relying and federated
 * parties.
 *
 * Relying Party (RP) An entity that relies upon a subject’s authenticator(s) and credentials or an IDP's assertion of a
 * subject’s identity, typically to process a transaction or to grant access to information or a system. In this case,
 * the RP is the Interaction Studio app.
 */
interface ValueMetadata {
    /**
     * The name of the entity that issues or creates the initial attribute value.
     *
     * The Origin element conveys the name of the entity that established the initial attribute value. This may or
     * may not be an authoritative entity, or the provider; if, for example, the AP generates the attribute value
     * through a derivation process, then the AP would be the origin. The key distinction between the origin and the
     * provider is the act of initially generating, capturing, or provisioning the attribute's value, rather than
     * just asserting the attribute's value to an RP.
     *
     * e.g. CRM, PointOfSaleSystem, FulfillmentSystem, or LoyaltySystem
     */
    origin?: string;
    /**
     * The name of the entity that is providing the attribute.
     *
     * This specifies the name of the entity that supplies the attribute value to the RP. This does not have to be
     * the AP itself. This element enables RPs to understand and evaluate the source of the individual attribute
     * values that may be included in a bundle of attributes. For example, if a full service credential provider
     * generates an assertion with several identity attributes provided by multiple APs, the {@code provider}
     * element enables the RP to understand, at a granular level, where each has come from and determine whether or
     * not that value can be used for access to specific resources. In instances where a single attribute is
     * asserted directly to the RP, this element may be omitted since the assertion itself will carry the provider
     * information as well as a certificate or digital signature.
     *
     * e.g. CsvUserEtlJob:user-20190528.csv.gz
     */
    provider?: string;
    /**
     * the ID of the Interaction Studio gear that updated the attribute, or null if it was not updated by a gear
     */
    gearId?: string;
    /**
     * The date and time when the attribute value was last updated
     */
    lastUpdated?: Date;
    /**
     * The date and time when the attribute value was last verified as being true and belonging to the specified
     * individual
     */
    lastVerified?: Date;
    /**
     * Metadata relevant or pertaining to the security classification of a given attribute's value
     */
    classification?: Classification;
}

type Classification = "Sensitive" | "PersonallyIdentifiable" | "NonSensitive";
```

### Immutability

You may have noticed many `readonly` modifiers throughout the attribute TypeScript declarations. Attribute collections are mutable - you can add, replace, and remove attributes.  However, each attribute, once created, is immutable.  This is to keep `value` and its optional associated `metadata` in sync - you must define them together.  Currently, you cannot replace/remove  an entire collection of attributes - instead you can mutate the existing collection.

### Example Usages

#### Creating Attributes

```ts
function mutatingUserAttributes(user: User) : void {
    // Removing
    delete user.attributes.nameToDelete;
    delete user.attributes['bracketStyle'];

    // Adding
    user.attributes.someString = { value: 'hi' };
    user.attributes['someInt'] = { value: 123, metadata: { lastUpdated: new Date(), classification: 'NonSensitive' } };
    user.attributes.someDecimal = { value: 123.45 };
    user.attributes['someDate'] = { value: new Date(322376400000) };
    user.attributes.someBool = { value: false };
    user.attributes['someObject'] = { value: { one: 'one', two: 2, zilch: null, when: new Date() } };
    user.attributes.someArray = { value: [ 'one', 2, null, new Date() ] };

    // Building up complex object to add
    let val: any = {}; // Not as AttributeValue yet since it's readonly
    val.firstName = "Bart"
    val.lastName = "Simpson"
    val.phones = { cell: "555-5555", work: "867-5309" };
    val.favoriteNumbers = [ 3, 7, 42 ];
    val.nesting = { a: ['b'], c: { d: [ 'e'] }}
    let meta: ValueMetadata = {};
    meta.classification = "NonSensitive";
    meta.lastUpdated = new Date();
    user.attributes.complexObject = { value: val, metadata: meta };

    // Building up complex array to add
    val = [];
    val.push('Abigail');
    val.push({ cell: "555-1234"});
    val.push([ 2, 4, 6]);
    val.push({ a: ['b'], c: { d: [ 'e', ['f']] }});
    val.push([ { one: 2, three: { four: [ 5, [ 6, 7]]}}, [ 8, { nine: 10}]]);
    meta = {};
    meta.origin = "someOrigin";
    meta.lastUpdated = new Date();
    user.attributes.complexArray = { value: val, metadata: meta };
}
```

#### Reading Attributes

```ts
// Not fully safe examples:
function readingUserAttributeValuesUnsafely(user: User): void {
    // Assuming attribute present and correct value type, simply casting:
    let str: string = user.attributes.someString.value as string;

    // Checking attribute present, then assuming correct value type and casting.
    // Using undefined to represent not-present:
    let num: number|undefined = !user.attributes.someNum ? undefined: user.attributes.someNum.value as number;
}

// Fully safe examples:
function readingUserAttributeValuesSafely(user: User): void {
    // Handling all cases:
    if (!user.attributes.someString) {
        // Attribute not present
    } else if (user.attributes.someString.value === null) {
        // Attribute value null
    } else if (typeof user.attributes.someString.value !== "string") {
        // Attribute value not a string
    } else {
        const str: string = user.attributes.someString.value as string;
    }

    // Checking attribute present and value type.
    // Using undefined to represent not-present or undesired-type:
    let bool: boolean|undefined =
        (user.attributes.someBool && (typeof user.attributes.someBool.value === "boolean"))
        ? user.attributes.someBool.value as boolean : undefined;

    // Similar but as an if statement:
    if (user.attributes.someBool && (typeof user.attributes.someBool.value === "boolean")) {
        const bool: boolean = user.attributes.someBool.value as boolean;
    }

    // Similar but using guard functions:

    if (user.attributes.someBool && isBoolean(user.attributes.someBool.value)) {
        const bool: boolean = user.attributes.someBool.value;
    }

    if (user.attributes.someDate && isDate(user.attributes.someDate.value)) {
        const date: Date = user.attributes.someDate.value;
    }

    if (user.attributes.complexObject && isObject(user.attributes.complexObject.value)) {
        const obj: AttributeValueObject = user.attributes.complexObject.value;
        if (isString(obj.aString)) {
            const str: string = obj.aString;
        }
        if (isDate(obj.aDate)) {
            const date: Date = obj.aDate;
        }
    }

    if (user.attributes.complexArray && isArray(user.attributes.complexArray.value)) {
        const arr: AttributeValueArray = user.attributes.complexArray.value;
        if (arr.length > 0 && isNumber(arr[0])) {
            const num: number = arr[0] as number;
        }
    }
}

// Guard functions:

// Currently the platform may provide a Date-like proxy, with getTime() etc. So avoid `instanceof`.
function isDate(value: AttributeValue): value is Date {
    return typeof value === "object" && typeof (value as Date).getTime === "function";
}

function isArray(value: AttributeValue): value is AttributeValueArray {
    return Array.isArray(value);
}

function isObject(value: AttributeValue): value is AttributeValueObject {
    return typeof value === "object" && !isArray(value) && !isDate(value);
}

function isBoolean(value: AttributeValue): value is boolean {
    return typeof value === "boolean";
}

function isString(value: AttributeValue): value is string {
    return typeof value === "string";
}

function isNumber(value: AttributeValue): value is number {
    return typeof value === "number";
}

function isNull(value: AttributeValue): value is null {
    return value === null;
}
```
