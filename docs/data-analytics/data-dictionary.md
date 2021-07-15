---
path: '/data-analytics/data-dictionary'
title: 'Data Dictionary'
tags: []
---
    
## TOC
[campaign](#table-campaign) | [category](#table-category) | [country](#table-country) | [dimension](#table-dimension) | [experience](#table-experience) | [goal\_completion\_attribution](#table-goal_completion_attribution) | [goal\_completion\_attribution\_promoted\_item](#table-goal_completion_attribution_promoted_item) | [goal\_completion\_line\_item](#table-goal_completion_line_item) | [location](#table-location) | [order\_line\_item](#table-order_line_item) | [order\_line\_item\_attribute](#table-order_line_item_attribute) | [product](#table-product) | [product\_attribute](#table-product_attribute) | [product\_category](#table-product_category) | [product\_dimension](#table-product_dimension) | [segment](#table-segment) | [segment\_membership](#table-segment_membership) | [survey\_question](#table-survey_question) | [survey\_response](#table-survey_response) | [user](#table-user) | [user\_alias](#table-user_alias) | [user\_attribute](#table-user_attribute) | [user\_campaign\_state](#table-user_campaign_state) | [user\_click](#table-user_click) | [user\_daily\_stat](#table-user_daily_stat) | [user\_daily\_stat\_action](#table-user_daily_stat_action) | [user\_daily\_stat\_item\_stat\_category](#table-user_daily_stat_item_stat_category) | [user\_daily\_stat\_item\_stat\_dimension](#table-user_daily_stat_item_stat_dimension) | [user\_daily\_stat\_item\_stat\_product](#table-user_daily_stat_item_stat_product) | [user\_daily\_stat\_item\_stat\_promotion](#table-user_daily_stat_item_stat_promotion) | [user\_dismissal](#table-user_dismissal) | [user\_engagement\_history](#table-user_engagement_history) | [user\_goal\_completion](#table-user_goal_completion) | [user\_impression](#table-user_impression) | [user\_order](#table-user_order) | [user\_order\_attribute](#table-user_order_attribute) | [user\_search](#table-user_search) | [user\_user\_visit\_lookup](#table-user_user_visit_lookup) | [user\_visit](#table-user_visit) | [user\_visit\_detail](#table-user_visit_detail)
## *Table campaign*

Campaigns contain experiences you design to personalize the interaction a visitor has with your website or application.

| Field Name        | Data Type                   | Description                                                | Example values                  |
|-------------------|-----------------------------|------------------------------------------------------------|---------------------------------|
| id                | Char(5)                     | Unique identifier for a campaign                           |                                 |
| "type"            | Varchar(32)                 | Campaign type implies marketing channel                    | Web, MobileData, TriggeredEmail |
| created\_ts        | Timestamp without time zone | UTC date and time the campaign was created                 |                                 |
| updated\_ts        | Timestamp without time zone | UTC date and time the campaign was most recently updated   |                                 |
| last\_published\_ts | Timestamp without time zone | UTC date and time the campaign was most recently published |                                 |
| name              | Varchar(100)                | Name of the campaign                                       |                                 |
| state             | Varchar(100)                | Campaign state                                             | Published, Testing, Disabled    |

### Indexes:
* PRIMARY KEY campaign\_pkey ON campaign USING btree (id)

### Referenced by:
* TABLE "experience" CONSTRAINT "experience\_campaign\_id\_fkey" FOREIGN KEY (campaign\_id) REFERENCES campaign(id)
* TABLE "user\_campaign\_state" CONSTRAINT "user\_campaign\_state\_campaign\_id\_fkey" FOREIGN KEY (campaign\_id) REFERENCES campaign(id)



### Example Query

```sql

/* Find campaigns published in the recent 7 days */
SELECT id,
       name
FROM campaign
WHERE last_published_ts >= CURRENT_DATE - 7;
   
```


[back to top](#toc)

---
## *Table category*

In a hierarchical catalog, items belong to a category. This table stores the category IDs and names. In Interaction Studio, there is more than one way to represent a hierarchy. First, categories can be fully specified by the category ID itself (for example, "womensapparel|denim|highrise"). A hierarchy can be derived from the path. Second, you can use the parent\_id field in the category. Products can belong to many categories. How you build categories will depend on your integration, but the data structure supports both approaches.

Encoding the hierarchy in the ID is simpler. The parent ID can be used to handle the case when the hierarchy is sometimes not available but the short, non-hierarchical ID is consistently available in the various channels and systems that refer to the categories.

| Field Name    | Data Type                   | Description                                                                                      | Example values   |
|---------------|-----------------------------|--------------------------------------------------------------------------------------------------|------------------|
| id            | Varchar(200)                | Unique identifier for the category                                                               |                  |
| name          | Varchar(200)                | Name of the category                                                                             |                  |
| is\_department | Boolean                     | True when the category is a department (a higher level entity in the category hierarchy)         |                  |
| parent\_id     | Varchar(200)                | Parent category ID of this category                                                              |                  |
| created\_ts    | Timestamp without time zone | Date and time the category was created in the operational store, UTC                             |                  |
| updated\_ts    | Timestamp without time zone | Date and time of the most recent update in the operation store, UTC (as of the recent DW import) |                  |

### Indexes:
* PRIMARY KEY category\_pkey ON category USING btree (id)

### Referenced by:
* TABLE "product\_category" CONSTRAINT "product\_category\_category\_id\_fkey" FOREIGN KEY (category\_id) REFERENCES category(id)
* TABLE "user\_daily\_stat\_item\_stat\_category" CONSTRAINT "user\_daily\_stat\_item\_stat\_category\_category\_id\_fkey" FOREIGN KEY (category\_id) REFERENCES category(id)



### Example Query

```sql

/* Get the number of products in each category */
SELECT c.name,
       count(1) AS num_products
FROM product_category pc
	     LEFT OUTER JOIN category c ON c.id = pc.category_id
GROUP BY 1
ORDER BY count(1) DESC
   
```


[back to top](#toc)

---
## *Table country*

Country lookup table. Provides iso\_2 and iso\_3 codes and continent information. See: https://www.iban.com/country-codes for more information.

| Field Name           | Data Type                   | Description                                          | Example values         |
|----------------------|-----------------------------|------------------------------------------------------|------------------------|
| id                   | Varchar(200)                | Unique identifier for the country                    |                        |
| name                 | Varchar(100)                | Name of the country                                  |                        |
| updated\_ts           | Timestamp without time zone | Date and time of the most recent update to the table |                        |
| iso\_2\_code           | Char(2)                     | 2-character country code                             | AW, GB, US             |
| iso\_3\_code           | Char(3)                     | 3-character country code                             | ABW, GBR               |
| has\_regions          | Boolean                     | True if the country has regions                      |                        |
| continent\_code       | Varchar(100)                | Numeric continent code                               | 0, 1, 2, 3, 4, 5, 6, 7 |
| continent\_iso\_2\_code | Char(2)                     | ISO-2 standard code for a continent                  | NA, AS, EU             |

### Indexes:
* PRIMARY KEY country\_pkey ON country USING btree (id)



### Example Query

```sql

/* count the number of visits by country */
SELECT c.name,
       count(1) as num_visits
FROM country c
	     LEFT OUTER JOIN user_visit_detail v ON c.id = v.country_code
GROUP BY 1
ORDER BY count(1) DESC;
   
```


[back to top](#toc)

---
## *Table dimension*

Dimensions are name-value pairs associated with items (usually products). Typical defaults are brand, color, style, and gender. Custom dimensions are also available. The combination of (id, dimension\_type) is unique

| Field Name     | Data Type                   | Description                                                                                            | Example values              |
|----------------|-----------------------------|--------------------------------------------------------------------------------------------------------|-----------------------------|
| id             | Varchar(200)                | Identifier for this dimension. The combination of id and dimension\_type uniquely identify a dimension. |                             |
| dimension\_type | Varchar(200)                | Built-in and custom types are supported. Dimension types are capitalized.                              | Brand, Gender, Color, Style |
| name           | Varchar(200)                | The label used when displaying the dimension                                                           | Gucci, Mens                 |
| created\_ts     | Timestamp without time zone | Date and time the dimension was created                                                                |                             |
| updated\_ts     | Timestamp without time zone | Date and time this dimension was most recently updated                                                 |                             |

### Indexes:
* UNIQUE INDEX dimension\_id\_key ON dimension USING btree (id, dimension\_type)

### Referenced by:
* TABLE "product\_dimension" CONSTRAINT "product\_dimension\_dimension\_id\_fkey" FOREIGN KEY (dimension\_id, dimension\_type) REFERENCES dimension(id, dimension\_type)



### Example Query

```sql

/* Get frequency and recency for stored dimension values */
SELECT dimension_type,
       count(1) AS num_names,
       max(updated_ts) AS most_recent_update
FROM dimension
GROUP BY 1;
   
```


[back to top](#toc)

---
## *Table experience*

Campaigns contain experiences. You can create different personalization results within the same campaign using experiences. For example, suppose you wish to create a campaign for first time visitors to your site, but you want to show a different message to visitors from Boston and San Francisco because you have events coming up in those two cities. You would create 3 experiences in the same campaign: one for visitors from Boston, one for visitors from San Francisco, and one for visitors from everywhere else. Then, you can use rules to restrict the visibility of these experiences to the target group of visitors.

| Field Name   | Data Type    | Description                               | Example values        |
|--------------|--------------|-------------------------------------------|-----------------------|
| id           | Char(5)      | Unique identifier for this experience     |                       |
| name         | Varchar(100) | Name of the experience                    |                       |
| display\_mode | Varchar(100) | Presentation detail about the experience. | Personalize, Redirect |
| campaign\_id  | Char(5)      | Associated Campaign ID                    |                       |

### Indexes:
* PRIMARY KEY experience\_pkey ON experience USING btree (id)

### Referenced by:
* TABLE "goal\_completion\_attribution" CONSTRAINT "goal\_completion\_attribution\_experience\_id\_fkey" FOREIGN KEY (experience\_id) REFERENCES experience(id)
* TABLE "goal\_completion\_attribution\_promoted\_item" CONSTRAINT "goal\_completion\_attribution\_promoted\_item\_experience\_id\_fkey" FOREIGN KEY (experience\_id) REFERENCES experience(id)
* TABLE "user\_click" CONSTRAINT "user\_click\_experience\_id\_fkey" FOREIGN KEY (experience\_id) REFERENCES experience(id)
* TABLE "user\_dismissal" CONSTRAINT "user\_dismissal\_experience\_id\_fkey" FOREIGN KEY (experience\_id) REFERENCES experience(id)
* TABLE "user\_impression" CONSTRAINT "user\_impression\_experience\_id\_fkey" FOREIGN KEY (experience\_id) REFERENCES experience(id)



### Example Query

```sql

/* Get a list of campaigns and experiences that are published */
SELECT c.name AS campaign_name,
       e.name AS experience_name
FROM campaign c
	     INNER JOIN experience e ON c.id = e.campaign_id
WHERE c.state = 'Published';
   
```


[back to top](#toc)

---
## *Table goal\_completion\_attribution*

Stores the timestamp for the campaign goal in UTC. This table is important when reconstructing or analyzing campaign performance. Use the last\_age\_seconds field to decide whether to attribute a goal completion to a campaign.

| Field Name        | Data Type                   | Description                                                                                                      | Example values                         |
|-------------------|-----------------------------|------------------------------------------------------------------------------------------------------------------|----------------------------------------|
| user\_id           | Varchar(200)                | User who satisfied the goal criteria                                                                             |                                        |
| goal\_ts           | Timestamp without time zone | Timestamp of the goal completion in UTC                                                                          |                                        |
| goal\_id           | Varchar(256)                | One of: segment id or purchase (p)                                                                               | p                                      |
| experience\_id     | Varchar(256)                | The experience associated with the goal completion                                                               |                                        |
| user\_group        | Char(1)                     | Per experience test (D) and control (C)                                                                          | C, D                                   |
| promotion\_id      | Varchar(200)                | The promotion associated with the goal completion                                                                |                                        |
| direct\_revenue    | Numeric(18,4)               | Revenue associated with the promoted item of the goal                                                            |                                        |
| event\_type        | Varchar(50)                 | The event type                                                                                                   | click, send, impression                |
| first\_age\_seconds | Integer                     | First age is the time between the first attributable event and the goal time.                                    |                                        |
| first\_time\_ts     | Timestamp without time zone | Timestamp for the first attributable event, UTC                                                                  |                                        |
| last\_age\_seconds  | Integer                     | Last age is the time between the most recent attributable event and the goal time.                               |                                        |
| last\_time\_ts      | Timestamp without time zone | Timestamp for the most recent attributable event, UTC                                                            |                                        |
| browser           | Varchar(256)                | Client browser application                                                                                       | Safari, Chrome, Firefox, IE, other     |
| device            | Varchar(256)                | Client hardware device                                                                                           | mobile, computer, tablet, other        |
| engagement        | Varchar(256)                | Engagement for the associated user                                                                               | low, medium, high                      |
| has\_purchased     | Boolean                     | Set when the user makes a purchase                                                                               |                                        |
| is\_anonymous      | Boolean                     | True when the user is not named (identified)                                                                     |                                        |
| is\_firsttime      | Boolean                     | True when the completion represents a first encounter with a visitor                                             |                                        |
| os                | Varchar(256)                | Operating system - x is "Mac OS X"                                                                               | x, windows, linux, ios, android, other |
| platform          | Varchar(256)                | Platform used in the goal completion (email, web)                                                                | Email, Web                             |
| source            | Varchar(256)                | For web events, source is Web. For customers who use the mobile sdk, each mobile app would be a separate source. |                                        |

### Indexes:
* UNIQUE INDEX goal\_completion\_attribution\_user\_id\_key ON goal\_completion\_attribution USING btree (user\_id, goal\_id, goal\_ts, experience\_id, user\_group, event\_type)



### Example Query

```sql

/* count the users eligible for goal completion with a 1-day window */

SELECT count(DISTINCT user_id)
FROM goal_completion_attribution
WHERE experience_id = '{EXPERIENCE_ID}'
	AND event_type = 'impression'
	AND last_age_seconds > 60 * 60 * 24;

   
```


[back to top](#toc)

---
## *Table goal\_completion\_attribution\_promoted\_item*

If a goal completion has an associated promoted item, you find it in this table. Use (user\_id, goal\_id, goal\_ts) to join back to the other goal tables.

| Field Name    | Data Type                   | Description                                                                                                                                       | Example values   |
|---------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| user\_id       | Varchar(200)                | User associated with the goal completion                                                                                                          |                  |
| goal\_ts       | Timestamp without time zone | Timestamp of the goal completion in UTC                                                                                                           |                  |
| goal\_id       | Varchar(256)                | The goal id stores different value types; for segment goals, the goal id is the segment id; for purchase goals, the goal id is the character 'p'. | p                |
| experience\_id | Varchar(256)                | Experience associated with the goal completion                                                                                                    |                  |
| user\_group    | Char(1)                     | Per experience control (C) and test (D)                                                                                                           | C, D             |
| item\_id       | Varchar(256)                | Unique identifier for the promoted item                                                                                                           |                  |
| item\_type     | Varchar(256)                | For example, "p" to indicate product                                                                                                              | p                |
| attributes    | Varchar(256)                | A list of attributes for the promoted item. The underlying data is JSON, stored here as a string.                                                 |                  |
| updated\_ts    | Timestamp without time zone | UTC Date and time of the most recent update of this promoted item attribution                                                                     |                  |
| revenue       | Numeric(18,4)               | Revenue resulting from the goal completion                                                                                                        |                  |

### Indexes:
* UNIQUE INDEX goal\_completion\_attribution\_promoted\_item\_user\_id\_key ON goal\_completion\_attribution\_promoted\_item USING btree (user\_id, goal\_id, goal\_ts, experience\_id, user\_group, item\_type, item\_id)



### Example Query

```sql

/* Get total revenue, users, and products associated with goal completions for an experience */

SELECT sum(revenue) AS total_revenue,
       count(DISTINCT user_id) AS num_users,
       count(DISTINCT item_id) AS num_products
FROM goal_completion_attribution_promoted_item
WHERE experience_id = '{EXPERIENCE_ID}';

   
```


[back to top](#toc)

---
## *Table goal\_completion\_line\_item*

For goal completions that are associated with a purchase, this table stores the line items associated with that purchase.

| Field Name    | Data Type                   | Description                                                                           | Example values   |
|---------------|-----------------------------|---------------------------------------------------------------------------------------|------------------|
| user\_id       | Varchar(200)                | User associated with the order                                                        |                  |
| goal\_ts       | Timestamp without time zone | Timestamp of the goal completion in UTC                                               |                  |
| goal\_id       | Varchar(256)                | "p" for purchase or a segment id                                                      | p                |
| line\_item\_num | Smallint                    | Datawarehouse internal enumeration (does not have an analog in the operational store) |                  |
| item\_id       | Varchar(256)                | Line item                                                                             |                  |
| item\_type     | Varchar(256)                | Typically 'p' for product                                                             | p                |
| price         | Numeric(18,4)               | Item price                                                                            |                  |
| quantity      | Smallint                    | Item quantity                                                                         |                  |

### Indexes:
* UNIQUE INDEX goal\_completion\_line\_item\_user\_id\_key ON goal\_completion\_line\_item USING btree (user\_id, goal\_id, goal\_ts, line\_item\_num)



### Example Query

```sql

/* average revenue per line item in goals */
SELECT avg(price * quantity)
FROM goal_completion_line_item;

   
```


[back to top](#toc)

---
## *Table location*

Use this table to de-reference geographical information stored at the visit or user level, (e.g., city\_code, metro\_code, postal\_code, latitude, or longitude) and convert it to human-readable labels.

| Field Name     | Data Type                   | Description                                                                       | Example values     |
|----------------|-----------------------------|-----------------------------------------------------------------------------------|--------------------|
| id             | Varchar(200)                | Unique identifier for the location                                                |                    |
| latitude       | Numeric(9,6)                | Latitude associated with the location                                             | 40.7134, 37.0547   |
| longitude      | Numeric(9,6)                | Longitude associated with the location                                            | -111.889, -91.5281 |
| postal\_code    | Varchar(10)                 | Postal code associated with the location                                          | 02143              |
| city\_code      | Integer                     | Code for the city associated with the location                                    |                    |
| city\_name      | Varchar(100)                | Name of the city associated with the location                                     |                    |
| metro\_code     | Integer                     | Metro code associated with the location. Metro codes for US are Nielsen DMA codes |                    |
| region\_code    | Varchar(10)                 | Region code associated with the location                                          | UT, WI             |
| country\_code   | Varchar(100)                | 2-letter Country code associated with the location                                | US, IT, KR         |
| continent\_code | Varchar(100)                | 2-letter continent code associated with the location                              | NA, SA             |
| updated\_ts     | Timestamp without time zone | UTC date and time of the most recent update to the location                      |                    |

### Indexes:
* PRIMARY KEY location\_pkey ON "location" USING btree (id)

[back to top](#toc)

---
## *Table order\_line\_item*

Stores the line items for a given order. Use (user\_id, order\_id) when joining back to order.

| Field Name       | Data Type                   | Description                                                                           | Example values   |
|------------------|-----------------------------|---------------------------------------------------------------------------------------|------------------|
| user\_id          | Varchar(200)                | User id associated with the order                                                     |                  |
| order\_id         | Varchar(256)                | Unique identifier for the order associated with the line item                         |                  |
| line\_item\_num    | Smallint                    | Datawarehouse internal enumeration (does not have an analog in the operational store) |                  |
| item\_type        | Varchar(256)                | Type of item. Usually product.                                                        |                  |
| item\_id          | Varchar(256)                | The item (usually product) associated with the order                                  |                  |
| price            | Numeric(18,4)               | Unit price                                                                            |                  |
| quantity         | Smallint                    | Item quantity for this line item                                                      |                  |
| currency         | Varchar(256)                | Currency associated with the order                                                    |                  |
| sku              | Varchar(256)                | Stock Keeping Unit associated with the line item                                      |                  |
| order\_created\_ts | Timestamp without time zone | UTC date and time when the order was created                                          |                  |

### Indexes:
* PRIMARY KEY order\_line\_item\_pkey ON order\_line\_item USING btree (user\_id, order\_id, item\_id, order\_created\_ts, line\_item\_num)

[back to top](#toc)

---
## *Table order\_line\_item\_attribute*

Provides attribute name-value pairs associated with line items, as well as provenance.

| Field Name                      | Data Type    | Description                                                                                                          | Example values                                           |
|---------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------|
| user\_id                         | Varchar(200) | User associated with the order                                                                                       |                                                          |
| order\_id                        | Varchar(256) | Unique order identifier                                                                                              |                                                          |
| line\_item\_num                   | Smallint     | Generated in DW, a 1-based sequence value for this line item                                                         |                                                          |
| item\_type                       | Varchar(256) | Type type of item                                                                                                    |                                                          |
| item\_id                         | Varchar(256) | Unique identifier for the associated item                                                                            |                                                          |
| attribute\_name                  | Varchar(256) | The name of the attribute                                                                                            | quantity\_shipped                                         |
| attribute\_value                 | Varchar(256) | The value of the attribute                                                                                           |                                                          |
| value\_metadata\_origin           | Varchar(256) | The name of the entity that issues or creates the initial attribute value.                                           | CRM, PointOfSaleSystem, FulfillmentSystem, LoyaltySystem |
| value\_metadata\_provider         | Varchar(256) | The name of the entity that is providing the attribute.                                                              | CsvUserEtlJob:user-20190528.csv.gz                       |
| value\_metadata\_gear\_id          | Varchar(256) | ID of the Interaction Studio gear that updated the attribute, or null if it was not updated by a gear                          |                                                          |
| value\_metadata\_last\_updated\_ts  | Varchar(256) | UTC date and time when the attribute was last updated                                                                |                                                          |
| value\_metadata\_last\_verified\_ts | Varchar(256) | UTC date and time when the attribute value was last verified as being true and belonging to the specified individual |                                                          |
| value\_metadata\_classification   | Varchar(256) | Metadata relevant or pertaining to the security classification of a given attribute's value                          |                                                          |

### Indexes:
* PRIMARY KEY order\_line\_item\_attribute\_pkey ON order\_line\_item\_attribute USING btree (user\_id, order\_id, item\_type, item\_id, line\_item\_num, attribute\_name)

[back to top](#toc)

---
## *Table product*

Stores product name and price.

| Field Name      | Data Type                   | Description                                                                    | Example values        |
|-----------------|-----------------------------|--------------------------------------------------------------------------------|-----------------------|
| id              | Varchar(200)                | Unique identifier for the product                                              |                       |
| name            | Varchar(200)                | Name of the product                                                            |                       |
| description     | Varchar(500)                | Description of the product                                                     |                       |
| list\_price      | Real                        | The list price of the product                                                  |                       |
| price           | Real                        | Unit price displayed to an anonymous user when promoting the product           |                       |
| created\_ts      | Timestamp without time zone | UTC date and time the product created in Interaction Studio system                       |                       |
| updated\_ts      | Timestamp without time zone | UTC date and time the product was most recently updated in the Interaction Studio system |                       |
| promotion\_state | Varchar(50)                 | Metadata concerning product eligibility and prioritization                     | Excluded, Prioritized |
| url             | Varchar(2048)               | URL for the product                                                            |                       |
| image\_url       | Varchar(2048)               | URL for the product image                                                      |                       |

### Indexes:
* PRIMARY KEY product\_pkey ON product USING btree (id)

### Referenced by:
* TABLE "product\_attribute" CONSTRAINT "product\_attribute\_product\_id\_fkey" FOREIGN KEY (product\_id) REFERENCES product(id)
* TABLE "product\_category" CONSTRAINT "product\_category\_product\_id\_fkey" FOREIGN KEY (product\_id) REFERENCES product(id)
* TABLE "product\_dimension" CONSTRAINT "product\_dimension\_product\_id\_fkey" FOREIGN KEY (product\_id) REFERENCES product(id)
* TABLE "user\_daily\_stat\_item\_stat\_product" CONSTRAINT "user\_daily\_stat\_item\_stat\_product\_product\_id\_fkey" FOREIGN KEY (product\_id) REFERENCES product(id)

[back to top](#toc)

---
## *Table product\_attribute*

Stores attribute (name-value) data about products, as well as provenance for the attribute value.

| Field Name                      | Data Type    | Description                                                                                                          | Example values                     |
|---------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|------------------------------------|
| product\_id                      | Varchar(200) | Unique identifier for the product                                                                                    |                                    |
| attribute\_name                  | Varchar(256) | The name of the attribute                                                                                            |                                    |
| attribute\_value                 | Varchar(256) | The value of the attribute                                                                                           |                                    |
| value\_metadata\_origin           | Varchar(256) | The name of the entity that issues or creates the initial attribute value
Exampe values: CRM, PointOfSaleSystem, FulfillmentSystem, LoyaltySystem                                                                                                                      |                                    |
| value\_metadata\_provider         | Varchar(256) | The name of the entity that is providing the attribute                                                               | CsvUserEtlJob:user-20190528.csv.gz |
| value\_metadata\_gear\_id          | Varchar(256) | ID of the Interaction Studio gear that updated the attribute, or null if it was not updated by a gear                          |                                    |
| value\_metadata\_last\_updated\_ts  | Varchar(256) | UTC date and time when the attribute was last updated                                                                |                                    |
| value\_metadata\_last\_verified\_ts | Varchar(256) | UTC date and time when the attribute value was last verified as being true and belonging to the specified individual |                                    |
| value\_metadata\_classification   | Varchar(256) | Metadata relevant or pertaining to the security classification of a given attribute's value                          |                                    |

### Indexes:
* PRIMARY KEY product\_attribute\_pkey ON product\_attribute USING btree (product\_id, attribute\_name)

[back to top](#toc)

---
## *Table product\_category*

A product can belong to more than one (fully-qualified) category. This table stores product-category relationships.

| Field Name   | Data Type    | Description                        | Example values   |
|--------------|--------------|------------------------------------|------------------|
| product\_id   | Varchar(256) | Unqiue identifier for the product  |                  |
| category\_id  | Varchar(256) | Unique identifier for the category |                  |

### Indexes:
* UNIQUE INDEX product\_category\_product\_id\_key ON product\_category USING btree (product\_id, category\_id)

[back to top](#toc)

---
## *Table product\_dimension*

Logically the (dimension\_type, dimension\_id) are a name-value pair associated with a product. Common dimensions include gender, brand, and style. Custom dimensions are also available.

| Field Name     | Data Type    | Description                                              | Example values       |
|----------------|--------------|----------------------------------------------------------|----------------------|
| product\_id     | Varchar(200) | Unique identifier for a product                          |                      |
| dimension\_id   | Varchar(200) | The value of a dimension                                 | Gucci, Red           |
| dimension\_type | Varchar(200) | The name of a dimension. Dimension names are capitalized | Brand, Gender, Style |

### Indexes:
* UNIQUE INDEX product\_dimension\_product\_id\_key ON product\_dimension USING btree (product\_id, dimension\_type, dimension\_id)

[back to top](#toc)

---
## *Table segment*

An Interaction Studio segment is a real-time grouping of accounts or individuals based on a set of criteria you define. Segment updates in the operational store happen in real-time, so any membership changes occur immediately, even during the same visit. Exports to this table, if done at all, are done once per day. Segment export is enabled for individual segments by request, on the condition that the segment is small enough to export in this manner.

| Field Name   | Data Type                   | Description                                     | Example values   |
|--------------|-----------------------------|-------------------------------------------------|------------------|
| id           | Varchar(10)                 | Unique identifier for the segment               |                  |
| name         | Varchar(200)                | Name of the segment                             |                  |
| is\_goal      | Boolean                     | True when segment is associated with a goal.    |                  |
| entity\_type  | Varchar(200)                | The type of entity associated with this segment | Account, User    |
| created\_ts   | Timestamp without time zone | UTC date and time of creation                   |                  |
| updated\_ts   | Timestamp without time zone | UTC date and time of recent update              |                  |

### Indexes:
* PRIMARY KEY segment\_pkey ON segment USING btree (id)

### Referenced by:
* TABLE "segment\_membership" CONSTRAINT "segment\_membership\_segment\_id\_fkey" FOREIGN KEY (segment\_id) REFERENCES segment(id)

[back to top](#toc)

---
## *Table segment\_membership*

User-segment associations. For B2C, segments are associated with users.

| Field Name   | Data Type                   | Description                                               | Example values   |
|--------------|-----------------------------|-----------------------------------------------------------|------------------|
| user\_id      | Varchar(200)                | Segment member                                            |                  |
| segment\_id   | Varchar(200)                | Unique identifier for the segment                         |                  |
| joined\_ts    | Timestamp without time zone | UTC date and time of segment join event                   |                  |
| left\_ts      | Timestamp without time zone | UTC date and time of segment leave event                  |                  |
| is\_active    | Boolean                     | True if user is currently an active member of the segment |                  |
| updated\_dt   | Timestamp without time zone | UTC date and time of most recent update                   |                  |

### Indexes:
* UNIQUE INDEX segment\_membership\_user\_id\_key ON segment\_membership USING btree (user\_id, segment\_id, joined\_ts)

[back to top](#toc)

---
## *Table survey\_question*

Survey questions and associated metadata

| Field Name        | Data Type                   | Description                                                                  | Example values   |
|-------------------|-----------------------------|------------------------------------------------------------------------------|------------------|
| survey\_id         | Varchar(6)                  | Unique identifier for the survey                                             |                  |
| survey\_name       | Varchar(200)                | Name of the survey                                                           |                  |
| survey\_updated\_ts | Timestamp without time zone | UTC date and time of the most recent update to the question                  |                  |
| page\_name         | Varchar(200)                | Page name on which the question appears                                      |                  |
| page\_num          | Integer                     | Internal sequence number for the page                                        |                  |
| element\_name      | Varchar(200)                | Name of the element in which the question appears. Pages have many elements. |                  |
| element\_title     | Varchar(200)                | Title of the element in which the question appears                           |                  |
| element\_type      | Varchar(200)                | The type of element - text, radiobutton, etc.                                |                  |
| element\_id        | Varchar(200)                | Unique identifier for the element in which the question appears              |                  |
| element\_num       | Integer                     | Internal sequence enumerating the elements on a page                         |                  |
| question\_id       | Varchar(200)                | Unique identifier for the question                                           |                  |

### Indexes:
* UNIQUE INDEX survey\_question\_survey\_id\_key ON survey\_question USING btree (survey\_id, question\_id, element\_id, page\_num, element\_num)

[back to top](#toc)

---
## *Table survey\_response*



| Field Name     | Data Type                   | Description                                                                           | Example values   |
|----------------|-----------------------------|---------------------------------------------------------------------------------------|------------------|
| survey\_id      | Varchar(6)                  | Unique identifier for the survey                                                      |                  |
| started\_ts     | Timestamp without time zone | UTC date and time the survery was started                                             |                  |
| user\_id        | Varchar(200)                | Unique identifier for the user taking the survey                                      |                  |
| question\_id    | Varchar(200)                | Unique identifier for the survey question                                             |                  |
| survey\_session | Varchar(200)                | hash of survey\_id, started\_ts, user\_id, which can help group questions into a session |                  |
| response\_ts    | Timestamp without time zone | UTC date and time the response was made                                               |                  |
| answer         | Varchar(2048)               | Long text field that captures the survey answer                                       |                  |

### Indexes:
* UNIQUE INDEX survey\_response\_user\_id\_key ON survey\_response USING btree (user\_id, survey\_id, question\_id, response\_ts)

[back to top](#toc)

---
## *Table user*

User is the center of the Interaction Studio data model. Much of the data about users is stored in tables other than this one; this table stores top-level information associated with a user. Fields prefixed with "loc\_" refer to location informaton. Location codes can be looked up in the Location table. Fields prefixed with "or\_" refer to the original referring event for this user.

| Field Name                    | Data Type                   | Description                                                                                                                                                                                                                                                               | Example values                                |
|-------------------------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| id                            | Varchar(200)                | Unique id for the user                                                                                                                                                                                                                                                    |                                               |
| id\_mod\_100                    | Integer                     | An offset from 0-99 used to create random partitions in the audience                                                                                                                                                                                                      |                                               |
| first\_activity\_ts             | Timestamp without time zone | UTC date and time the user was first observed                                                                                                                                                                                                                             |                                               |
| last\_activity\_ts              | Timestamp without time zone | UTC date and time the user was most recently observed                                                                                                                                                                                                                     |                                               |
| engagement\_score              | Integer                     | The engagement score is an index that tracks how engaged your site visitors and application users are based on several different factors including visit information, actions, key performance indicators (KPIs), and segments. Higher values indicate deeper engagement. |                                               |
| engagement\_dt                 | Date                        | The time the engagement score was computed.                                                                                                                                                                                                                               |                                               |
| created\_ts                    | Timestamp without time zone | UTC date and time this record was created                                                                                                                                                                                                                                 |                                               |
| loc\_device\_provided           | Boolean                     | True if the location (loc\_ fields) was provided by a device (e.g. obtained via a GPS retrieval), or false if this is an approximate location determined via geocoding of an IP address                                                                                    |                                               |
| loc\_ip\_address                | Varchar(100)                | IP Address associated with the user location                                                                                                                                                                                                                              |                                               |
| loc\_continent\_key             | Varchar(100)                | 2-letter continent code associated with the user location                                                                                                                                                                                                                 | NA, AS                                        |
| loc\_country\_code              | Char(2)                     | An ISO 3166-1 alpha-2 (http://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2) country code (e.g. "US")location                                                                                                                                                                  | US                                            |
| loc\_country\_numeric\_code      | Smallint                    | ISO-3166-1 numeric country code (e.g. 840 for United States)                                                                                                                                                                                                              |                                               |
| loc\_region\_code               | Integer                     | Location's region code. Note - does not correspond to location(region\_code)                                                                                                                                                                                               |                                               |
| loc\_metro\_code                | Integer                     | Location's metro code, corresponds to location(metro\_code) For the US, this will be the Nielsen Designated Market Area (DMA) code                                                                                                                                         |                                               |
| loc\_city\_code                 | Integer                     | Location's city code, corresponds to location(city\_code)                                                                                                                                                                                                                  |                                               |
| loc\_postal\_code               | Varchar(10)                 | Location's postal code                                                                                                                                                                                                                                                    |                                               |
| loc\_isp                       | Varchar(100)                | Internet Service Provider associated with a location                                                                                                                                                                                                                      | Telstra Internet, Comcast, Verizon            |
| loc\_organization              | Varchar(100)                | Organization associated with a location (can be null)                                                                                                                                                                                                                     |                                               |
| loc\_naics\_code                | Integer                     | NAICS industry code                                                                                                                                                                                                                                                       |                                               |
| or\_medium                     | Varchar(50)                 | Original referrer medium - how the user first arrived                                                                                                                                                                                                                     | SEARCH, EMAIL, SOCIAL, DIRECT                 |
| or\_referrer\_source            | Varchar(50)                 | Source of the original referrer                                                                                                                                                                                                                                           | Google, Bing, Yahoo!, Facebook, Pintrest      |
| or\_query\_terms                | Varchar(512)                | Search terms for user who first arrived via search                                                                                                                                                                                                                        |                                               |
| or\_referrer\_domain            | Varchar(100)                | Domain of the original referrer                                                                                                                                                                                                                                           | google.com, bing.com, google.de, facebook.com |
| or\_referrer\_reverse\_subdomain | Varchar(200)                | Full host associated with the original referrer, reversed Used operationally                                                                                                                                                                                             | mx.com.google.www                             |
| is\_anonymous                  | Boolean                     | True when this user is not identifiable                                                                                                                                                                                                                                   |                                               |
| lifetime\_value                | Real                        | Total amount this user has spent                                                                                                                                                                                                                                          |                                               |
| loc\_city                      | Varchar(256)                | City name associated with the user's most recent location                                                                                                                                                                                                                 |                                               |
| loc\_latitude                  | Real                        | Latitude associated with the user's most recent location                                                                                                                                                                                                                  |                                               |
| loc\_longitude                 | Real                        | Longitude assocaited with the user's most recent location                                                                                                                                                                                                                 |                                               |
| loc\_timezone                  | Varchar(256)                | Timezone associated with the user's most recent location                                                                                                                                                                                                                  |                                               |
| updated\_ts                    | Timestamp without time zone | UTC date and time the user was most recently updated                                                                                                                                                                                                                      |                                               |
| loc\_state\_province\_code       | Varchar(256)                | Alphanumeric code associated with the user's most recent location                                                                                                                                                                                                         | KY, PAC                                       |
| or\_referring\_url              | Varchar(2048)               | URL associated with the original referrer                                                                                                                                                                                                                                 | https://www.google.com/                       |
| or\_landing\_url                | Varchar(2048)               | URL of the first page the user saw                                                                                                                                                                                                                                        |                                               |
| email\_address                 | Varchar(100)                | Email address associated with the user, if available                                                                                                                                                                                                                      |                                               |

### Indexes:
* PRIMARY KEY user\_pkey ON "user" USING btree (id)

### Referenced by:
* TABLE "goal\_completion\_attribution\_promoted\_item" CONSTRAINT "goal\_completion\_attribution\_promoted\_item\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "goal\_completion\_attribution" CONSTRAINT "goal\_completion\_attribution\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "goal\_completion\_line\_item" CONSTRAINT "goal\_completion\_line\_item\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "order\_line\_item\_attribute" CONSTRAINT "order\_line\_item\_attribute\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "order\_line\_item" CONSTRAINT "order\_line\_item\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "segment\_membership" CONSTRAINT "segment\_membership\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_alias" CONSTRAINT "user\_alias\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_attribute" CONSTRAINT "user\_attribute\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_campaign\_state" CONSTRAINT "user\_campaign\_state\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_click" CONSTRAINT "user\_click\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat\_action" CONSTRAINT "user\_daily\_stat\_action\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat\_item\_stat\_category" CONSTRAINT "user\_daily\_stat\_item\_stat\_category\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat\_item\_stat\_dimension" CONSTRAINT "user\_daily\_stat\_item\_stat\_dimension\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat\_item\_stat\_product" CONSTRAINT "user\_daily\_stat\_item\_stat\_product\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat\_item\_stat\_promotion" CONSTRAINT "user\_daily\_stat\_item\_stat\_promotion\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_daily\_stat" CONSTRAINT "user\_daily\_stat\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_dismissal" CONSTRAINT "user\_dismissal\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_engagement\_history" CONSTRAINT "user\_engagement\_history\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_goal\_completion" CONSTRAINT "user\_goal\_completion\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_impression" CONSTRAINT "user\_impression\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_order\_attribute" CONSTRAINT "user\_order\_attribute\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_order" CONSTRAINT "user\_order\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_search" CONSTRAINT "user\_search\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)
* TABLE "user\_visit" CONSTRAINT "user\_visit\_user\_id\_fkey" FOREIGN KEY (user\_id) REFERENCES "user"(id)

[back to top](#toc)

---
## *Table user\_alias*

This table contains the IDs of any anonymous users that have been merged into a given user. It will also contain any non-anonymous IDs they used to have. This occurs when userIDs are changed to a new format, or simply a new ID.

| Field Name   | Data Type    | Description                       | Example values   |
|--------------|--------------|-----------------------------------|------------------|
| user\_id      | Varchar(200) | Unique identifier for a user      |                  |
| anon\_alias   | Varchar(200) | User Id associated with this user |                  |

### Indexes:
* UNIQUE INDEX user\_alias\_user\_id\_key ON user\_alias USING btree (user\_id, anon\_alias)

[back to top](#toc)

---
## *Table user\_attribute*

Name-value pairs associated with a user (many different use cases - ids, segment support, etc.)

| Field Name   | Data Type                   | Description                                                | Example values   |
|--------------|-----------------------------|------------------------------------------------------------|------------------|
| user\_id      | Varchar(200)                | Unique identifier for the user                             |                  |
| updated\_ts   | Timestamp without time zone | UTC date and time this attribute was most recently updated |                  |
| name         | Varchar(256)                | Name of the attribute                                      |                  |
| value        | Varchar(256)                | Value of the attribute                                     |                  |

### Indexes:
* PRIMARY KEY user\_attribute\_pkey ON user\_attribute USING btree (user\_id, name, updated\_ts)

[back to top](#toc)

---
## *Table user\_campaign\_state*

Summary information about a user or campaign. This is a mirror of an operational table that provides current state to the platform for up-to-the-minute reporting or other applications; less useful in a warehouse context, but does provide the state at batch time. Can be used to quickly confirm membership or perform other ad hoc tasks which are later migrated to fully fleshed-out queries.

| Field Name          | Data Type                   | Description                                                             | Example values   |
|---------------------|-----------------------------|-------------------------------------------------------------------------|------------------|
| user\_id             | Varchar(200)                | Unique identifier for the user                                          |                  |
| campaign\_id         | Varchar(256)                | Unique identifier for the campaign                                      |                  |
| last\_reset\_time\_ts  | Timestamp without time zone | UTC date and time of the most recent campaign reset                     |                  |
| experience\_id       | Varchar(256)                | Unique identifier for the experience                                    |                  |
| user\_group          | Varchar(256)                | The user group (control or test) associated with this user's experience |                  |
| num\_clicks          | Integer                     | Total number of clicks                                                  |                  |
| first\_click\_ts      | Timestamp without time zone | UTC date and time of the least recent click                             |                  |
| last\_click\_ts       | Timestamp without time zone | UTC date and time of the most recent click                              |                  |
| num\_impressions     | Integer                     | Total number of impressions                                             |                  |
| first\_impression\_ts | Timestamp without time zone | UTC date and time of the least recent impression                        |                  |
| last\_impression\_ts  | Timestamp without time zone | UTC date and time of the most recent impression                         |                  |

### Indexes:
* UNIQUE INDEX user\_campaign\_state\_user\_id\_key ON user\_campaign\_state USING btree (user\_id, campaign\_id, experience\_id, user\_group)

[back to top](#toc)

---
## *Table user\_click*

Stores click events associated with a campaign at the visit level. Event\_ts is associated with the most recent event.

| Field Name    | Data Type                   | Description                                                                       | Example values                         |
|---------------|-----------------------------|-----------------------------------------------------------------------------------|----------------------------------------|
| user\_id       | Varchar(200)                | Unique identifier for the user                                                    |                                        |
| experience\_id | Varchar(256)                | Unique identifier for the experience                                              |                                        |
| event\_ts      | Timestamp without time zone | UTC date and time of the most recent click in a session                               |                                        |
| user\_group    | Char(1)                     | The user group associated with the user and experience - control (C) or test (D)  |                                        |
| browser       | Varchar(256)                | Client browser application                                                        | Safari, Chrome, Firefox, IE, other     |
| device        | Varchar(256)                | Client hardware device                                                            | mobile, computer, tablet, other        |
| engagement    | Varchar(256)                | Engagement for the associated user                                                | low, medium, high                      |
| has\_purchased | Boolean                     | Set when the user makes a purchase                                                |                                        |
| click\_count   | Smallint                    | Total number of clicks for the session                                            |                                        |
| is\_anonymous  | Boolean                     | True when the user is not named (identified)                                      |                                        |
| is\_firsttime  | Boolean                     | True when this is a user's first visit                                            |                                        |
| os            | Varchar(256)                | Operating system - x is "Mac OS X"                                                | x, windows, linux, ios, android, other |
| platform      | Varchar(256)                | Platform associated with the click(s)                                             | Email, Web                             |
| source        | Varchar(256)                | For customers who use the mobile SDK, each mobile app would be listed as a separate source |                                        |

### Indexes:
* PRIMARY KEY user\_click\_pkey ON user\_click USING btree (user\_id, experience\_id, event\_ts)

[back to top](#toc)

---
## *Table user\_daily\_stat*

Aggregation of events at the (user, day) grain

| Field Name    | Data Type    | Description                                   | Example values   |
|---------------|--------------|-----------------------------------------------|------------------|
| stat\_date     | Date         | Date of the stat aggregation for this user    |                  |
| user\_id       | Varchar(200) | Unique identifier for the user                |                  |
| visit\_count   | Integer      | Number of times user visited on the stat\_date |                  |
| visit\_millis  | Bigint       | Total visit duration, in milliseconds         |                  |
| total\_actions | Integer      | Number of actions performed by the user       |                  |
| num\_pages     | Integer      | Number of pages the user viewed               |                  |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_stat\_date\_key ON user\_daily\_stat USING btree (stat\_date, user\_id)

[back to top](#toc)

---
## *Table user\_daily\_stat\_action*

Aggregation of events at the (user, day, action) grain

| Field Name   | Data Type    | Description                                                               | Example values                            |
|--------------|--------------|---------------------------------------------------------------------------|-------------------------------------------|
| stat\_date    | Date         | Date of the stat aggregation for this user                                |                                           |
| user\_id      | Varchar(200) | Unique identifier for the user                                            |                                           |
| action\_id    | Varchar(200) | Identifier for an action performed by the user                           | Home, Search, View Category, View Product |
| action\_count | Integer      | Total count of actions for this action on this day by the associated user |                                           |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_action\_stat\_date\_key ON user\_daily\_stat\_action USING btree (stat\_date, user\_id, action\_id)

[back to top](#toc)

---
## *Table user\_daily\_stat\_item\_stat\_category*

Aggregation of events at the (user, day, product category) grain

| Field Name         | Data Type     | Description                                                   | Example values   |
|--------------------|---------------|---------------------------------------------------------------|------------------|
| stat\_date          | Date          | Date associated with the stat                                 |                  |
| user\_id            | Varchar(200)  | Unique identifier for the user                                |                  |
| category\_id        | Varchar(200)  | Unique identifer for the category                             |                  |
| cart\_value         | Numeric(18,4) | Total value of items in the cart for this day, category, user |                  |
| num\_cart\_items     | Integer       | Count of items in the cart                                    |                  |
| num\_purchases      | Integer       | Count of purchases                                            |                  |
| out\_of\_stock\_views | Integer       | Count of views of products that were out of stock             |                  |
| purchase\_value     | Numeric(18,4) | Total value of purchased made for this user, category, date   |                  |
| recommended\_count  | Integer       | Number of items recommended to the user for this category     |                  |
| view\_time\_millis   | Integer       | Total view time for the category in milliseconds              |                  |
| view\_value         | Numeric(18,4) | The sum of the value of items viewed in this category         |                  |
| views              | Integer       | Number of views for this category, day, user                  |                  |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_item\_stat\_category\_stat\_date\_key ON user\_daily\_stat\_item\_stat\_category USING btree (stat\_date, user\_id, category\_id)

[back to top](#toc)

---
## *Table user\_daily\_stat\_item\_stat\_dimension*

Aggregation of events at the (user, day, product dimension) grain

| Field Name         | Data Type     | Description                                                    | Example values                  |
|--------------------|---------------|----------------------------------------------------------------|---------------------------------|
| stat\_date          | Date          | Date associated with the stat                                  |                                 |
| user\_id            | Varchar(200)  | Unique identifier for the user                                 |                                 |
| dimension\_type     | Varchar(256)  | The name of a dimension. Dimension names are capitalized.      | Brand, Gender, Style, ItemClass |
| dimension\_id       | Varchar(256)  | Unique identifer for the dimension                             |                                 |
| cart\_value         | Numeric(18,4) | Total value of items in the cart for this day, dimension, user |                                 |
| num\_cart\_items     | Integer       | Count of items in the cart                                     |                                 |
| num\_purchases      | Integer       | Count of purchases                                             |                                 |
| out\_of\_stock\_views | Integer       | Count of views of products that were out of stock              |                                 |
| purchase\_value     | Numeric(18,4) | Total value of purchased made for this user, dimension, date   |                                 |
| recommended\_count  | Integer       | Number of items recommended to the user for this dimension     |                                 |
| view\_time\_millis   | Integer       | Total view time for the dimension in milliseconds              |                                 |
| view\_value         | Numeric(18,4) | The sum of the value of items viewed in this dimension         |                                 |
| views              | Integer       | Number of views for this dimension, day, user                  |                                 |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_item\_stat\_dimension\_stat\_date\_key ON user\_daily\_stat\_item\_stat\_dimension USING btree (stat\_date, user\_id, dimension\_type, dimension\_id)

[back to top](#toc)

---
## *Table user\_daily\_stat\_item\_stat\_product*

Aggregation of events at the (user, day, product) grain

| Field Name         | Data Type     | Description                                                  | Example values   |
|--------------------|---------------|--------------------------------------------------------------|------------------|
| stat\_date          | Date          | Date associated with the stat                                |                  |
| user\_id            | Varchar(200)  | Unique identifier for the user                               |                  |
| product\_id         | Varchar(200)  | Unique identifer for the product                             |                  |
| cart\_value         | Numeric(18,4) | Total value of items in the cart for this day, product, user |                  |
| num\_cart\_items     | Integer       | Count of items in the cart                                   |                  |
| num\_purchases      | Integer       | Count of purchases                                           |                  |
| out\_of\_stock\_views | Integer       | Count of views of products that were out of stock            |                  |
| purchase\_value     | Numeric(18,4) | Total value of purchased made for this user, product, date   |                  |
| recommended\_count  | Integer       | Number of items recommended to the user for this product     |                  |
| view\_time\_millis   | Integer       | Total view time for the product in milliseconds              |                  |
| view\_value         | Numeric(18,4) | The sum of the value of items viewed in this product         |                  |
| views              | Integer       | Number of views for this product, day, user                  |                  |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_item\_stat\_product\_stat\_date\_key ON user\_daily\_stat\_item\_stat\_product USING btree (stat\_date, user\_id, product\_id)



### Example Query

```sql

/* top products using daily stats */
SELECT
	CASE WHEN pct_total > 1
		     THEN a.name
	     ELSE 'ALL OTHERS' END                          AS name,
	to_char(sum(revenue), 'FM$999,999,999,990D00') AS revenue,
	to_char(sum(pct_total), 'FM99.99%')            AS pct_total,
	sum(rnk)                                          rnk
FROM
	(
		SELECT
			p.name,
			sum(a.num_purchases * purchase_value) revenue,
			ratio_to_report(sum(a.num_purchases * purchase_value))
			OVER () * 100             AS          pct_total,
			row_number()
			OVER (
				ORDER BY revenue DESC ) AS          rnk
		FROM
			product p,
			user_daily_stat_item_stat_product a
		WHERE
				p.id = a.product_id
		GROUP BY 1
		ORDER BY sum(a.num_purchases * purchase_value) DESC
	) a
GROUP BY 1
ORDER BY rnk
   
```


[back to top](#toc)

---
## *Table user\_daily\_stat\_item\_stat\_promotion*

Aggregation of events at the (user, day, promotion) grain

| Field Name            | Data Type     | Description                                                          | Example values   |
|-----------------------|---------------|----------------------------------------------------------------------|------------------|
| stat\_date             | Date          | Date for the stat                                                    |                  |
| user\_id               | Varchar(200)  | Unique identifier for the user                                       |                  |
| promotion\_id          | Varchar(200)  | Unique identifier for the promotion                                  |                  |
| cart\_value            | Numeric(18,4) | Total value of items in the cart for this day, promotion, user       |                  |
| num\_cart\_items        | Integer       | Count of items in the cart                                           |                  |
| num\_purchases         | Integer       | Count of purchases                                                   |                  |
| out\_of\_stock\_views    | Integer       | Count of views of promotions associated with an out of stock product |                  |
| purchase\_value        | Numeric(18,4) | Total value of purchases made for this user, promotion, date         |                  |
| recommended\_count     | Integer       | Number of items recommended to the user for this promotion           |                  |
| view\_time\_millis      | Integer       | Total view time for the promotion in milliseconds                    |                  |
| view\_value            | Numeric(18,4) | The sum of the value of promotions viewed                            |                  |
| views                 | Integer       | Number of views by this user for this promotion and day              |                  |
| eligible\_for\_serving  | Integer       | Number of times this promotion was eligible to be served             |                  |
| requested\_for\_serving | Integer       | Number of times this promotion was requested to be served            |                  |
| served                | Integer       | Number of times this promotion was served                            |                  |

### Indexes:
* UNIQUE INDEX user\_daily\_stat\_item\_stat\_promotion\_stat\_date\_key ON user\_daily\_stat\_item\_stat\_promotion USING btree (stat\_date, user\_id, promotion\_id)

[back to top](#toc)

---
## *Table user\_dismissal*

Stores dismissal events associated with a campaign at the visit level. Event\_ts is associated with the most recent event.

| Field Name      | Data Type                   | Description                                                                       | Example values                         |
|-----------------|-----------------------------|-----------------------------------------------------------------------------------|----------------------------------------|
| user\_id         | Varchar(200)                | Unique identifier for the user                                                    |                                        |
| experience\_id   | Varchar(256)                | Unique identifier for the experience                                              |                                        |
| event\_ts        | Timestamp without time zone | UTC date and time of the most recent dismissal in a session.                            |                                        |
| user\_group      | Char(1)                     | The user group associated with the user and experience - control (C) or test (D)  |                                        |
| browser         | Varchar(256)                | Client browser application                                                        | Safari, Chrome, Firefox, IE, other     |
| device          | Varchar(256)                | Client hardware device                                                            | mobile, computer, tablet, other        |
| engagement      | Varchar(256)                | Engagement for the associated user                                                | low, medium, high                      |
| has\_purchased   | Boolean                     | Set when the user makes a purchase                                                |                                        |
| dismissal\_count | Smallint                    | Total number of dismissals for the session                                        |                                        |
| is\_anonymous    | Boolean                     | True when the user is not named (identified)                                      |                                        |
| is\_firsttime    | Boolean                     | True when this is a user's first visit                                            |                                        |
| os              | Varchar(256)                | Operating system - x is "Mac OS X"                                                | x, windows, linux, ios, android, other |
| platform        | Varchar(256)                | Platform associated with the dismissal(s)                                         | Email, Web                             |
| source          | Varchar(256)                | For customers who use the mobile sdk, each mobile app would be a separate source. |                                        |

### Indexes:
* PRIMARY KEY user\_dismissal\_pkey ON user\_dismissal USING btree (user\_id, experience\_id, event\_ts)

[back to top](#toc)

---
## *Table user\_engagement\_history*

Stores historical engagement values at the user grain

| Field Name   | Data Type    | Description                                                                                                                                                                                         | Example values   |
|--------------|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| user\_id      | Varchar(200) | Unique identifier for a user                                                                                                                                                                        |                  |
| "day"        | Date         | Date of the day tracked for engagement                                                                                                                                                              |                  |
| score        | Integer      | Engagement score is an integer reflecting a percentage from 0-125 and the value depends on account configuration. See: https://doc.evergage.com/display/EKB/Understand+Engagement+Score+Calculation |                  |

### Indexes:
* UNIQUE INDEX user\_engagement\_history\_user\_id\_key ON user\_engagement\_history USING btree (user\_id, "day")

[back to top](#toc)

---
## *Table user\_goal\_completion*

Since goal completions often involve a purchase, purchase data is stored at the top level in this table and is empty for non-purchase goal completions

| Field Name              | Data Type                   | Description                                                                       | Example values                         |
|-------------------------|-----------------------------|-----------------------------------------------------------------------------------|----------------------------------------|
| user\_id                 | Varchar(200)                | User who satisfied the goal criteria                                              |                                        |
| goal\_ts                 | Timestamp without time zone | Timestamp of the goal completion in UTC                                           |                                        |
| goal\_id                 | Varchar(256)                | One of: segment id or purchase (p)                                                | p                                      |
| order\_id                | Varchar(256)                | Unique identifier for the order. This comes from the customer.                    |                                        |
| order\_currency          | Varchar(256)                | Currency used to purchase                                                         |                                        |
| order\_line\_item\_count   | Smallint                    | Count of line items in the order                                                  |                                        |
| order\_total\_value       | Numeric(18,4)               | Sum of quantity * price for all line items                                        |                                        |
| order\_units\_count       | Smallint                    | Count of units in the order                                                       |                                        |
| order\_visit\_age\_minutes | Double precision            | Age of the vist at the time of the purchase goal completion                       |                                        |
| order\_visit\_time        | Timestamp without time zone | UTC date and time of the visit during which this order was made                   |                                        |
| browser                 | Varchar(256)                | Client browser application                                                        | Safari, Chrome, Firefox, IE, other     |
| device                  | Varchar(256)                | Client hardware device                                                            | mobile, computer, tablet, other        |
| engagement              | Varchar(256)                | Engagement for the associated user                                                | low, medium, high                      |
| has\_purchased           | Boolean                     | Set when the user makes a purchase                                                |                                        |
| is\_anonymous            | Boolean                     | True when the user is not named (identified)                                      |                                        |
| is\_first\_time           | Boolean                     | True when the completion represents a first encounter with a visitor              |                                        |
| os                      | Varchar(256)                | Operating system - x is "Mac OS X"                                                | x, windows, linux, ios, android, other |
| platform                | Varchar(256)                | Platform used in the goal completion (email, web)                                 | Email, Web                             |
| source                  | Varchar(256)                | For customers who use the mobile sdk, each mobile app would be a separate source. |                                        |

### Indexes:
* UNIQUE INDEX user\_goal\_completion\_user\_id\_key ON user\_goal\_completion USING btree (user\_id, goal\_id, goal\_ts)

[back to top](#toc)

---
## *Table user\_impression*

Stores impression events associated with a campaign at the visit level. Event\_ts is associated with the most recent event.

| Field Name       | Data Type                   | Description                                                                       | Example values                         |
|------------------|-----------------------------|-----------------------------------------------------------------------------------|----------------------------------------|
| user\_id          | Varchar(200)                | Unique identifier for the user                                                    |                                        |
| experience\_id    | Varchar(256)                | Unique identifier for the experience                                              |                                        |
| event\_ts         | Timestamp without time zone | UTC date and time of the most recent impression in a session.                           |                                        |
| user\_group       | Char(1)                     | The user group associated with the user and experience - control (C) or test (D)  |                                        |
| browser          | Varchar(256)                | Client browser application                                                        | Safari, Chrome, Firefox, IE, other     |
| device           | Varchar(256)                | Client hardware device                                                            | mobile, computer, tablet, other        |
| engagement       | Varchar(256)                | Engagement for the associated user                                                | low, medium, high                      |
| has\_purchased    | Boolean                     | Set when the user makes a purchase                                                |                                        |
| impression\_count | Smallint                    | Total number of impressions for the session                                       |                                        |
| is\_anonymous     | Boolean                     | True when the user is not named (identified)                                      |                                        |
| is\_firsttime     | Boolean                     | True when this is a user's first visit                                            |                                        |
| os               | Varchar(256)                | Operating system - x is "Mac OS X"                                                | x, windows, linux, ios, android, other |
| platform         | Varchar(256)                | Platform associated with the impression(s)                                        | Email, Web                             |
| source           | Varchar(256)                | For customers who use the mobile sdk, each mobile app would be a separate source. |                                        |

### Indexes:
* PRIMARY KEY user\_impression\_pkey ON user\_impression USING btree (user\_id, experience\_id, event\_ts)

[back to top](#toc)

---
## *Table user\_order*

Stores purchases. Only purchased orders are kept in the Data Warehouse, nothing open

| Field Name               | Data Type                   | Description                                                                                                            | Example values   |
|--------------------------|-----------------------------|------------------------------------------------------------------------------------------------------------------------|------------------|
| id                       | Varchar(100)                | Unique identifier for the order. This comes from the customer.                                                         |                  |
| user\_id                  | Varchar(200)                | Unique identifier for the user                                                                                         |                  |
| order\_num                | Smallint                    | 1-based sequence of orders for this user. Based on the array order in the operational store. (Not set by the customer) |                  |
| visit\_start\_ts           | Timestamp without time zone | Start time of the visit in which this order was placed                                                                 |                  |
| visit\_num                | Integer                     | Visit in which order create time falls w/in visit start/end.                                                           |                  |
| created\_ts               | Timestamp without time zone | UTC date and time at which the order was created                                                                       |                  |
| updated\_ts               | Timestamp without time zone | UTC date and time the order was most recently updated                                                                  |                  |
| purchased\_ts             | Timestamp without time zone | UTC date and time the purchase was made                                                                                |                  |
| visit\_age\_at\_purchase\_ms | Integer                     | Age of the session, in milliseconds, at purchase time                                                                  |                  |
| num\_items                | Integer                     | Number of items in the order                                                                                           |                  |
| num\_units                | Integer                     | Number of units (distinct items) in the order                                                                          |                  |
| total\_value              | Numeric(18,4)               | Total value of the purchase                                                                                            |                  |
| currency                 | Varchar(200)                | Currency used to make the purchase                                                                                     |                  |
| status                   | Varchar(200)                | Purchased only, other states not stored in the warehoure. (Open, Purchased, Canceled).                                 |                  |

### Indexes:
* PRIMARY KEY user\_order\_pkey ON user\_order USING btree (id)
* UNIQUE INDEX user\_order\_user\_id\_key ON user\_order USING btree (user\_id, created\_ts)

### Referenced by:
* TABLE "order\_line\_item\_attribute" CONSTRAINT "order\_line\_item\_attribute\_order\_id\_fkey" FOREIGN KEY (order\_id) REFERENCES user\_order(id)
* TABLE "order\_line\_item" CONSTRAINT "order\_line\_item\_order\_id\_fkey" FOREIGN KEY (order\_id) REFERENCES user\_order(id)
* TABLE "user\_order\_attribute" CONSTRAINT "user\_order\_attribute\_order\_id\_fkey" FOREIGN KEY (order\_id) REFERENCES user\_order(id)

[back to top](#toc)

---
## *Table user\_order\_attribute*

Name-value pairs associated with an order

| Field Name                      | Data Type    | Description                                                                                                          | Example values                     |
|---------------------------------|--------------|----------------------------------------------------------------------------------------------------------------------|------------------------------------|
| order\_id                        | Varchar(100) | Unique identifier for the order                                                                                      |                                    |
| user\_id                         | Varchar(200) | User who made the purchase                                                                                           |                                    |
| order\_num                       | Smallint     | Internally generated 1-based sequence indicating which order this is                                                 |                                    |
| attribute\_name                  | Varchar(256) | The name of the attribute                                                                                            |                                    |
| attribute\_value                 | Varchar(256) | The value of the attribute                                                                                           |                                    |
| value\_metadata\_origin           | Varchar(256) | The name of the entity that issues or creates the initial attribute value
Exampe values: CRM, PointOfSaleSystem, FulfillmentSystem, LoyaltySystem                                                                                                                      |                                    |
| value\_metadata\_provider         | Varchar(256) | The name of the entity that is providing the attribute                                                               | CsvUserEtlJob:user-20190528.csv.gz |
| value\_metadata\_gear\_id          | Varchar(256) | ID of the Interaction Studio gear that updated the attribute, or null if it was not updated by a gear                          |                                    |
| value\_metadata\_last\_updated\_ts  | Varchar(256) | UTC date and time when the attribute was last updated                                                                |                                    |
| value\_metadata\_last\_verified\_ts | Varchar(256) | UTC date and time when the attribute value was last verified as being true and belonging to the specified individual |                                    |
| value\_metadata\_classification   | Varchar(256) | Metadata relevant or pertaining to the security classification of a given attribute's value                          |                                    |

### Indexes:
* PRIMARY KEY user\_order\_attribute\_pkey ON user\_order\_attribute USING btree (user\_id, order\_id, attribute\_name)

[back to top](#toc)

---
## *Table user\_search*

User search terms (some normalization / tokenization is performed)

| Field Name         | Data Type                   | Description                                                | Example values   |
|--------------------|-----------------------------|------------------------------------------------------------|------------------|
| user\_id            | Varchar(200)                | User who performed the search                              |                  |
| term               | Varchar(200)                | Search term (tokenized)                                    |                  |
| most\_recent\_search | Timestamp without time zone | UTC date and time of the most recent search for this term  |                  |
| product\_clicks     | Integer                     | Count of product clicks associated with the search         |                  |
| purchase\_value     | Numeric(18,4)               | Total purchase value of orders associated with this search |                  |
| search\_count       | Integer                     | Total number of searches for this term                     |                  |

### Indexes:
* PRIMARY KEY user\_search\_pkey ON user\_search USING btree (user\_id, term)

[back to top](#toc)

---
## *Table user\_user\_visit\_lookup*

Use this table to join the user\_visit\_detail with other tables containing a user\_id. The user\_id in this table corresponds to the general notion of user\_id in the database, and user(id). The user\_visit\_key is the user\_id in the user\_visit\_detail table.

| Field Name     | Data Type    | Description                                     | Example values   |
|----------------|--------------|-------------------------------------------------|------------------|
| user\_id        | Varchar(200) | User id from the user table                     |                  |
| user\_id\_hashed | Varchar(200) | Hashed user id from the user\_visit\_detail table |                  |

### Indexes:
* UNIQUE INDEX user\_user\_visit\_lookup\_user\_id\_key ON user\_user\_visit\_lookup USING btree (user\_id, user\_id\_hashed)

[back to top](#toc)

---
## *Table user\_visit*

Stores information about the user visit

Fields that only appear in user visit: referring\_campaign\_id, weather, previous\_path, temperature, referrer, num\_pageviews, num\_events, last\_event\_ts, visit\_num

Fields that appear in both user visit and user visit detail: is\_anonymous, browser, engagement, source, utm\_parameters, device, is\_firsttime, has\_purchased, platform, start\_ts, \_dw\_created\_ts, os, user\_id

Note that the user\_id user\_visit\_detail is a hash value of the user ID in this table; use user\_user\_lookup to join

| Field Name            | Data Type                   | Description                                                                                                                                                                      | Example values                         |
|-----------------------|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------|
| user\_id               | Varchar(200)                | User id associated with the visit                                                                                                                                                |                                        |
| visit\_num             | Smallint                    | Internally generated sequence value in the visit history - which visit this was.                                                                                                 |                                        |
| num\_events            | Integer                     | Count of events for the visit                                                                                                                                                    |                                        |
| previous\_path         | Varchar(100)                | The most recent page view action for this visit (operation field)                                                                                                                |                                        |
| num\_pageviews         | Integer                     | Count of pages viewed in the visit                                                                                                                                               |                                        |
| referring\_campaign\_id | Varchar(32)                 | External campaign that drove the visit                                                                                                                                           |                                        |
| device                | Varchar(100)                | Client hardware device                                                                                                                                                           | mobile, computer, tablet, other        |
| engagement            | Varchar(100)                | Engagement for the associated user                                                                                                                                               | low, medium, high                      |
| os                    | Varchar(100)                | Operating system - x is "Mac OS X"                                                                                                                                               | x, windows, linux, ios, android, other |
| browser               | Varchar(100)                | Client browser application                                                                                                                                                       | Safari, Chrome, Firefox, IE, other     |
| platform              | Varchar(100)                | Platform used in the goal completion (email, web)	E                                                                                                                                                                                  | Email, Web                             |
| temperature           | Varchar(100)                | Recorded temprature at the physical location of the user during the visit time                                                                                                        |                                        |
| weather               | Varchar(100)                | Recorded weather at the physical location of the user during the visit time                                                                                                      |                                        |
| referrer              | Varchar(2048)               | HTTP referrer for the visit                                                                                                                                                      |                                        |
| start\_ts              | Timestamp without time zone | UTC date and time the visit began                                                                                                                                                |                                        |
| last\_event\_ts         | Timestamp without time zone | UTC date and time of the most recent event in the visit                                                                                                                          |                                        |
| utm\_parameters        | Varchar(100)                | Urchin Tracking Module (UTM) parameters identify the marketing campaign that refers traffic to a specific website. These are the parameters associated with the visit's referrer |                                        |
| has\_purchased         | Boolean                     | Set when the user makes a purchase                                                                                                                                               |                                        |
| is\_anonymous          | Boolean                     | True when the user is not named (identified)                                                                                                                                     |                                        |
| is\_first\_time         | Boolean                     | True when this is the first visit                                                                                                                                                |                                        |
| source                | Varchar(10)                 | For customers who use the mobile sdk, each mobile app would be a separate source                                                                                                 |                                        |

### Indexes:
* PRIMARY KEY user\_visit\_pkey ON user\_visit USING btree (user\_id, start\_ts)
* UNIQUE INDEX user\_visit\_user\_id\_key ON user\_visit USING btree (user\_id, visit\_num)

### Referenced by:
* TABLE "user\_order" CONSTRAINT "user\_order\_user\_id\_visit\_num\_fkey" FOREIGN KEY (user\_id, visit\_num) REFERENCES user\_visit(user\_id, visit\_num)
* TABLE "user\_order" CONSTRAINT "user\_order\_user\_id\_visit\_start\_ts\_fkey" FOREIGN KEY (user\_id, visit\_start\_ts) REFERENCES user\_visit(user\_id, start\_ts)

[back to top](#toc)

---
## *Table user\_visit\_detail*

Stores additional information about the user visit - location and referrer are usually of interest.

The following fields appear only in the visit detail, not the visit: region\_code, referrer\_domain, revenue, city\_code, metro\_code, longitude, industry\_code, referring\_campaign, country\_code, action\_count, page\_view\_count, referrer\_subdomain, referrer\_url, referrer\_terms, referrer\_source, latitude, duration\_minutes, referrer\_landing\_url, referrer\_medium

Fields that appear in both this table and user\_visit: is\_anonymous, browser, engagement, source, utm\_parameters, device, is\_firsttime, has\_purchased, platform, start\_ts, \_dw\_created\_ts, os, user\_id

Note the user\_id in this table is a hash value on the user\_id in user\_visit

| Field Name           | Data Type                   | Description                                                                                                                                                                      | Example values                                |
|----------------------|-----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------|
| user\_id\_hashed       | Varchar(200)                | Hashcode of the user\_id (id from the user table) associated with this visit                                                                                                      |                                               |
| start\_ts             | Timestamp without time zone | UTC date and time the visit started                                                                                                                                              |                                               |
| referrer\_medium      | Varchar(100)                | How the user first arrived                                                                                                                                                       | SEARCH, EMAIL, SOCIAL, DIRECT                 |
| referrer\_source      | Varchar(100)                | Source of the visit                                                                                                                                                              | Google, Bing, Yahoo!, Facebook, Pintrest      |
| referrer\_terms       | Varchar(255)                | Search terms for associated with the referral                                                                                                                                    |                                               |
| referrer\_domain      | Varchar(100)                | Domain of the visit referrer                                                                                                                                                     | google.com, bing.com, google.de, facebook.com |
| referrer\_subdomain   | Varchar(100)                | Referrer host with sub-domain                                                                                                                                                    | www.google.com.mx                             |
| referrer\_url         | Varchar(2048)               | URL associated with the visit referrer                                                                                                                                           |                                               |
| referrer\_landing\_url | Varchar(2048)               | URL of the first page the user saw                                                                                                                                               |                                               |
| utm\_parameters       | Varchar(100)                | Urchin Tracking Module (UTM) parameters identify the marketing campaign that refers traffic to a specific website. These are the parameters associated with the visit's referrer |                                               |
| latitude             | Numeric(9,6)                | Latitude associated with the visitor's physical location                                                                                                                          |                                               |
| longitude            | Numeric(9,6)                | Longitude associated with the visitor's physical location                                                                                                                                     |                                               |
| country\_code         | Varchar(100)                | 2-letter Country code associated with the visit                                                                                                                                  |                                               |
| region\_code          | Varchar(100)                | Region code associated with the visit                                                                                                                                            |                                               |
| metro\_code           | Integer                     | Metro code associated with the visit. Metro codes for US are Nielsen DMA codes                                                                                                   |                                               |
| city\_code            | Integer                     | Code for the city associated with the visit                                                                                                                                      |                                               |
| industry\_code        | Integer                     | NAICS industry code                                                                                                                                                              |                                               |
| referring\_campaign   | Varchar(32)                 | External campaign that drove the visit                                                                                                                                           |                                               |
| device               | Varchar(100)                | Client hardware device                                                                                                                                                           | mobile, computer, tablet, other               |
| engagement           | Varchar(100)                | Engagement for the associated user                                                                                                                                               | low, medium, high                             |
| os                   | Varchar(100)                | Operating system - x is "Mac OS X"                                                                                                                                               | x, Windows, Linux, iOs, Android, other        |
| browser              | Varchar(100)                | Client browser application                                                                                                                                                       | Safari, Chrome, Firefox, IE, other            |
| platform             | Varchar(100)                | Platform (email, web)                                                                                                                                                            | Email, Web, Mobile                            |
| has\_purchased        | Boolean                     | Set when the user makes a purchase                                                                                                                                               |                                               |
| is\_anonymous         | Boolean                     | True when the user is not named (identified)                                                                                                                                     |                                               |
| is\_first\_time        | Boolean                     | True when this is the first visit                                                                                                                                                |                                               |
| source               | Varchar(10)                 | For customers who use the mobile SDK, each mobile app would be a separate source                                                                                                 |                                               |
| duration\_minutes     | Double precision            | Time spent in the visit, in minutes                                                                                                                                              |                                               |
| revenue              | Numeric(18,4)               | Total revenue associated with purchases made during the visit                                                                                                                    |                                               |
| action\_count         | Integer                     | Count of actions performed this visit                                                                                                                                            |                                               |
| page\_view\_count      | Integer                     | Count of pages viewed this visit                                                                                                                                                 |                                               |

### Indexes:
* PRIMARY KEY user\_visit\_detail\_pkey ON user\_visit\_detail USING btree (user\_id\_hashed, start\_ts)

[back to top](#toc)

---
