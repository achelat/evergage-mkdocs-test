---
path: '/data-model/catalog'
title: 'Catalog'
tags: ['Catalog', 'Data Model']
---

The Interaction Studio platform can model key business objects as items within a shadow catalog. The configuration of this happens within the Interaction Studio [catalog setup](https://doc.evergage.com/display/EKB/Catalog+Setup) screens.


### Items

Items, such as products or articles represent key business objects that a user can interact with. These items can be configured with a name, labels, [custom attributes](./attributes) and linked dimensions. User behavior is tracked against items as well as against their dimensions.

### Attributes

Catalog items and item types can also have attributes that you can customize based on business requirements. You can configure custom attributes for items in addition to those already available to reflect supplementary information about the item such as price, date added, or image alternate text for products or author, subject, or keyword for blogs and articles.

A catalog item's attributes must satisfy the following conditions to be promoted and appear in Interaction Studio Recommendations:

* The item must not be archived.
* The `promotionState` attribute must not be set to `Excluded`.
* The `expiration` date (if any) must not be before the current date.
* The `published` date (if any) must not be after the current date.

In addition to satisfying the conditions listed above, all catalog items must have values for their `name` and `url` attributes. Additionally, items of the type `Product` must also have a `price` greater than zero, an `inventoryCount` greater than zero, and a value for their `imageUrl` attribute.

### Dimensions

Dimensions represent categorical information about items and can have custom attributes.
When [creating dimensions via the UI](https://doc.evergage.com/display/EKB/Catalog+Setup#CatalogSetup-AddDimensions) or [importing dimensions via ETL](https://doc.evergage.com/display/EKB/Dimension+ETL), each Dimension type is configured with a unique name. This name must be unique within the dataset and is permanent.  Similarly, when creating an instance of a dimension, an id is created that is permanent.  The dimension type name and the dimension instance id cannot be modifed or used for other dimension configuration within the dataset.  When a dimension is deleted/archived it remains in the system but is no longer accessible.  When designing your data model, plan on the fact any name of a dimenstion type and id of a dimension instance cannot be used again in the future.  This is one of the reasons we recommend the use of a 'development' or 'test' dataset when initialy configuring Interaction Studio - it provides the flexibility required to load data and then change your data model without placing any restrictions on your production environment.


### Item Actions

Actions model a user's interaction with an item. Examples include:

* View
* Add to Cart
* Purchase
