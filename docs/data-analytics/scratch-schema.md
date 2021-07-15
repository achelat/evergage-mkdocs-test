---
path: '/data-analytics/scratch-schema'
title: 'Scratch Schema'
tags: []
---

the Interaction Studio data warehouse supports the use of "scratch" tables for analysis, reporting, or investigation. In addition, scratch tables can be used as a data source by the Interaction Studio ETL system. Using the ETL system and scratch tables, data scientists can load machine learning model scoring outputs from their data science workbench back into the Interaction Studio production database to enrich user profile attributes. Customers can add and drop scratch tables in their data warehouse that follow the scratch schema specications described in this article. 
## ETL Scratch Tables

The Data Warehouse scratch tables temporarily store Data Science workbench model score outputs for loading back into Interaction Studio user profile attributes. Interaction Studio Data Warehouse supports the ingestion of data written to customer's scratch schema. Tables in the scratch schema can be treated as data sources for the Interaction Studio ETL system. 

Multi-channel operation is enabled when user identities can be matched with the Interaction Studio identity resolution system. Catalog data best support multi-channel operation when identities for online and offline product and dimension identities match.

## *User Tables*

Table Name Format: **schema\_user\_{token}\_YYYYMMDD\_HHMMSS** 

Example: `retail_user_20200101_093000`. Optionally you may include a descriptive token in the name: `retail_user_email06_20200101_090000`.


**N.B.** You must supply some form of user id, so at least one of the optional user or email address fields below is required to process a row.

In order to be able to process updates, include a created\_ts field as shown below. This way the ETL can process rows created since the last run. Instead of updating attributes in a scratch table, use a DELETE and an INSERT so that the created\_ts field is reset, or include the created\_ts in your UPDATE.



### Requirements and Schema

| Field Name  | Minimum Requirements      | Example Values       | Data Type    |   |
|-------------|---------------------------|----------------------|--------------|---|
| user_id         | Optional. Must match the format of the configured default user ID type for this feed. Since a user can have multiple identities, user ids can also be constructed from other identifying information in the record (email\_address, attributes or other user\_id: fields). If none of these methods result in a user ID, this record will not be loaded.  | jdoe, john.doe@example.com, c2e384084c8ac233            |  VARCHAR(120)   ||
| user_id:        | Optional. Column titles are the token 'user\_id' followed by colon followed by the user ID type. Values must match the format of the specified user ID type. Otherwise, processed like user\_id. | "user\_id:named": "jdoe", "user\_id:marketo\_id": "29237102" | VARCHAR(120)   ||
| account_id | Optional. If provided, a string value of the account Id to associate with the user. Must be lowercase. This is only available with Interaction Studio B2B functionality enabled. Contact your customer success representative for details. | example industries, example.com | VARCHAR(120)||
| display_name    | Optional. Display name for user.      | Bob Newhart     | VARCHAR(1023)  ||
| email_address   | Optional. Valid email address. If provided, used as a user identity of the type emailAddress. | user@example.com, bob.smith@example.com | VARCHAR(1023)  ||
| attribute: | Optional. The custom attributes for this user. Column titles are the word 'attribute' followed by colon followed by the attribute name. Attributes are singled-valued and always interpreted as strings. Attributes can be used to construct user ids. | "attribute:lastname": Gina, "attribute:firstname: Torres | VARCHAR(1023)    ||
| created\_ts | **Required.** Records when the row was created. || TIMESTAMP WITHOUT TIME ZONE NOT NULL DEFAULT getdate() ||



