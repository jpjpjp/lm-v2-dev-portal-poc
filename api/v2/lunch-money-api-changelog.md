<style>
/* Lunch Money API Changelog Styles - Matching support.lunchmoney.app aesthetic */

/* CSS Variables for consistent theming */
:root {
  --lm-text-primary: #1f2937;
  --lm-text-secondary: #6b7280;
  --lm-text-muted: #9ca3af;
  --lm-border: #e5e7eb;
  --lm-border-light: #f3f4f6;
  --lm-bg: #ffffff;
  --lm-bg-subtle: #f9fafb;
  --lm-code-bg: #f6f8fa;
  --lm-code-border: #e1e4e8;
  --lm-shadow: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);
  --lm-shadow-lg: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  --lm-radius: 8px;
  --lm-radius-sm: 6px;
}

/* Typography - System font stack */
body {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  color: var(--lm-text-primary);
  line-height: 1.7;
  font-size: 16px;
}

/* Headings */
h1 {
  font-size: 32px;
  font-weight: 700;
  color: #0f172a;
  line-height: 1.2;
  margin: 0 0 0.5em 0;
  letter-spacing: -0.02em;
}

h2 {
  font-size: 26px;
  font-weight: 650;
  color: #111827;
  line-height: 1.3;
  margin: 2em 0 0.75em 0;
  padding-bottom: 0.5em;
  border-bottom: 2px solid var(--lm-border);
  letter-spacing: -0.01em;
}

h3 {
  font-size: 20px;
  font-weight: 600;
  color: #1f2937;
  line-height: 1.4;
  margin: 1.75em 0 0.75em 0;
}

h4 {
  font-size: 18px;
  font-weight: 600;
  color: #374151;
  line-height: 1.4;
  margin: 1.5em 0 0.6em 0;
}

/* Paragraphs and text */
p {
  margin: 0.75em 0;
  color: var(--lm-text-primary);
  line-height: 1.7;
}

/* Lists */
ul, ol {
  margin: 0.75em 0;
  padding-left: 1.75em;
  color: var(--lm-text-primary);
}

li {
  margin: 0.5em 0;
  line-height: 1.7;
}

/* Links */
a {
  color: #2563eb;
  text-decoration: none;
  transition: color 0.2s ease;
}

a:hover {
  color: #1d4ed8;
  text-decoration: underline;
}

/* Code */
code {
  background: var(--lm-code-bg);
  border: 1px solid var(--lm-code-border);
  border-radius: 4px;
  padding: 0.2em 0.4em;
  font-size: 0.9em;
  font-family: "SF Mono", Monaco, "Cascadia Code", "Roboto Mono", Consolas, "Courier New", monospace;
  color: #e83e8c;
}

pre {
  background: var(--lm-code-bg);
  border: 1px solid var(--lm-code-border);
  border-radius: var(--lm-radius-sm);
  padding: 1em;
  overflow-x: auto;
  margin: 1em 0;
}

pre code {
  background: transparent;
  border: none;
  padding: 0;
  color: var(--lm-text-primary);
  font-size: 0.9em;
}

/* Badge/Pill styles */
.badge {
  display: inline-block;
  font-weight: 600;
  font-size: 11px;
  line-height: 1;
  padding: 5px 10px;
  border-radius: 999px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin-left: 8px;
  vertical-align: middle;
}

.badge-breaking {
  background: #fee2e2;
  color: #991b1b;
  border: 1px solid #fecaca;
}

.badge-deprecated {
  background: #fef3c7;
  color: #92400e;
  border: 1px solid #fde68a;
}

.badge-beta {
  background: #e0f2fe;
  color: #075985;
  border: 1px solid #bae6fd;
}

.badge-new {
  background: #ecfdf5;
  color: #065f46;
  border: 1px solid #a7f3d0;
}

/* Callout/Info boxes using blockquotes */
blockquote {
  border-left: 4px solid #93c5fd;
  background: #eff6ff;
  padding: 14px 18px;
  border-radius: var(--lm-radius-sm);
  margin: 1.25em 0;
  color: #1e293b;
}

blockquote.callout-warn {
  border-left-color: #fbbf24;
  background: #fffbeb;
  color: #78350f;
}

blockquote.callout-danger {
  border-left-color: #f87171;
  background: #fef2f2;
  color: #991b1b;
}

blockquote.callout-info {
  border-left-color: #60a5fa;
  background: #eff6ff;
  color: #1e3a8a;
}

blockquote.callout-tip {
  border-left-color: #10b981;
  background: #ecfdf5;
  color: #065f46;
}

blockquote p:first-child {
  margin-top: 0;
}

blockquote p:last-child {
  margin-bottom: 0;
}

blockquote strong {
  display: block;
  margin-bottom: 0.5em;
  font-weight: 600;
}

/* Section cards - wrapping major sections */
.section-card {
  background: var(--lm-bg);
  border: 1px solid var(--lm-border);
  border-radius: var(--lm-radius);
  box-shadow: var(--lm-shadow);
  padding: 24px 28px;
  margin: 2em 0;
  transition: box-shadow 0.2s ease;
}

.section-card:hover {
  box-shadow: var(--lm-shadow-lg);
}

.section-card h2 {
  border-bottom: none;
  padding-bottom: 0;
  margin-top: 0;
}

.section-card h3:first-child {
  margin-top: 0;
}

/* Horizontal rule */
hr {
  border: 0;
  border-top: 1px solid var(--lm-border);
  margin: 2.5em 0;
}

/* Table of contents styling */
nav[aria-label="Table of contents"] ul,
ul:has(> li > a[href^="#"]) {
  background: var(--lm-bg-subtle);
  border: 1px solid var(--lm-border);
  border-radius: var(--lm-radius);
  padding: 20px 24px;
  margin: 2em 0;
  list-style: none;
  padding-left: 0;
}

nav[aria-label="Table of contents"] li,
ul:has(> li > a[href^="#"]) li {
  margin: 0.4em 0;
  padding-left: 0;
}

nav[aria-label="Table of contents"] a,
ul:has(> li > a[href^="#"]) a {
  color: var(--lm-text-primary);
  text-decoration: none;
  display: block;
  padding: 4px 0;
}

nav[aria-label="Table of contents"] a:hover,
ul:has(> li > a[href^="#"]) a:hover {
  color: #2563eb;
  text-decoration: underline;
}

/* Spacing improvements */
section {
  margin: 2em 0;
}

/* Endpoint sections - special styling for endpoint lists */
.endpoint-list {
  background: var(--lm-bg-subtle);
  border-left: 3px solid #3b82f6;
  padding: 1em 1.5em;
  margin: 1em 0;
  border-radius: var(--lm-radius-sm);
}

.endpoint-list ul {
  margin: 0.5em 0;
}

/* Remove that test green div styling if it appears */
div[style*="color:green"] {
  display: none;
}
</style>

<!-- omit in toc -->
# Changes in the v2 version of the Lunch Money API
- [General changes:](#general-changes)
  - [Versioning](#versioning)
  - [Error Behavior](#error-behavior)
  - [Request Validation](#request-validation)
  - [Type change for IDs](#type-change-for-ids)
  - [Object property naming conventions](#object-property-naming-conventions)
  - [Successful `POST` requests](#successful-post-requests)
  - [Successful `PUT` requests](#successful-put-requests)
  - [Successful `DELETE` requests](#successful-delete-requests)
- [Endpoint Specific Changes](#endpoint-specific-changes)
  - [The User Object](#the-user-object)
  - [The Category Object](#the-category-object)
  - [categories endpoints](#categories-endpoints)
  - [The Tags Object](#the-tags-object)
  - [tags endpoints](#tags-endpoints)
  - [The Transaction Object](#the-transaction-object)
  - [transactions endpoints](#transactions-endpoints)
    - [Endpoints that return or operate on a single transaction](#endpoints-that-return-or-operate-on-a-single-transaction)
    - [Endpoints that return or operate on an array of transactions](#endpoints-that-return-or-operate-on-an-array-of-transactions)
    - [Endpoints for grouping or ungrouping transactions](#endpoints-for-grouping-or-ungrouping-transactions)
    - [Endpoints for splitting or unsplitting transactions](#endpoints-for-splitting-or-unsplitting-transactions)
    - [The Transaction Attachment Object](#the-transaction-attachment-object)
    - [Endpoints for managing file attachments](#endpoints-for-managing-file-attachments)
  - [The Recurring Object](#the-recurring-object)
  - [recurring endpoints](#recurring-endpoints)
  - [The Manual Account Object (formerly Asset Object)](#the-manual-account-object-formerly-asset-object)
  - [manual\_accounts endpoints](#manual_accounts-endpoints)
  - [The Plaid Account Object](#the-plaid-account-object)
  - [plaid\_accounts endpoints](#plaid_accounts-endpoints)


## General changes

### Versioning

The v2 Lunch Money API is versioned using a modified version of SEMVER as follows:
- The major version is the API version.  This will always be 2 for the v2 version of the API.   Any API changes which would cause existing applications to break would trigger a new major version release.
- The minor version represents the number of main endpoints the current version of the API supports.  For example, a version of the API that supports only the /me, /categories, and /transactions endpoints would have a minor version of 3.
- The revision number represents the number of updates since the last endpoint was added.  For example, each time changes are made to one of the existing three APIs as described above, the revision number will be bumped.

This initial public change log encapsulates the changes in the Lunch Money API from the v1 API to the initial public release of version v2.x.x of the v2 API.

This document encapsulates the sum of changes vis-a-vis the v1 version of the API as of the current release, but you can also view a [detailed version history](./version-history). 


### Error Behavior


- All Error responses will now come with a 4XX Response Code:
  - `400 - Bad Request` - there were one or more problems with the parameters and/or request body for the request
  - `401 - Unauthorized` - there was a problem with the token passed in via the Authorization header
  - `404 - Not Found` - the specified object was not found
  - `429 - Too Many Requests` - the client has made too many requests and must wait before making another request. 
    - Responses with this status will include a `Retry-After` header indicating the number of seconds to wait before making another API call.




- Error response bodies will be JSON objects with two fields:
  - `message` - an overall message describing the error
  - `errors` - an array of objects that describe one or more specific errors.  Each object has at least the following property:
    - `errMsg` - an error message about a specific query or path parameter or a request property
    - Objects in the `errors` array may include additional properties now or in the future.


- When an API returns a 4XX code, in general, no change to the data model will be made.  Some examples of this are:
  - If attempting to insert an array of multiple transactions, if only one transaction has an error, no transactions will be inserted.
    - Address the issues in the error response and retry the attempt without fear of creating duplicate transactions.
  - If attempting to modify a category group with invalid child categories, no changes will be made to the existing children in the category group.
    - Address the issues in the error response and retry the attempt.
  - When practical, if a request contains multiple errors, the error response will provide information for the  errors via the `errors` array returned in a `400 Bad Request` response.
    - It is not guaranteed however that all errors will be caught in a single request response.


### Request Validation
In general, requests are validated and will return 400 responses if:
 - Unexpected parameters are provided
 - Expected parameters are provided with an invalid value
 - Request bodies do not contain required properties
 - Request bodies contain expected properties with invalid values
 - Request bodies contain unexpected properties


### Type change for IDs
IDs are now specified as `integer` instead of `number` in the various request and response schemas to allow for better validation.

### Object property naming conventions
We have attempted to name (or rename) object properties to apply consistent behavior as follows:
- Properties that belong to the object (ie: `id`) will simply be called that property, and not use object_id (ie: `category_id`).
- Properties that belong to related objects will include the related object name as part of the property name (ie: the transaction object has a property `category_id`)
- Properties that refer to a time that an action took place on an object end with the word "at" (ie: `created_at`, `updated_at`, `archived_at`).
- Lists of IDs will be named to include the type of ids they are (ie: `tag_ids`).
- Objects that can include child objects of the same type (ie: category groups or split transactions) will include a property called `children` which will contain a list of the child objects.


### Successful `POST` requests


- Will now return a `201 Created` response and use the following rules for request and response bodies:
  - Request bodies are generally restricted to the "user-definable" properties of the object to be created
    - For example, a request body for a new transaction may include an `amount` and a `date` but may not include properties that are set by the system, such as `id` or `date_created`.
  - Response bodies will include the complete created object, including the user and "system-defined" properties of the created object.


### Successful `PUT` requests


- Will return a `200 OK` response and use the following rules for request and response bodies:
  - Request bodies must contain at least one or more "user-definable" properties to change.
  - Any "user-definable" property in a PUT will be changed in the specified object.
  - Any "system-defined" property in a PUT request will be tolerated but ignored.
  - Any other unexpected property will fail validation and return a 400 response.

The values for complex properties, such as objects and arrays that are set in the request body, will completely replace any previous value for these properties. Therefore, if you wish to, for example, add a new tag to a transaction or a new child category to a category group, you should first query the existing object and then update the property appropriately.  


- Because request bodies MAY optionally contain "system-definable" properties, developers may perform PUT requests using the following pattern:
  - Make a `GET /endpoint/{id}` request.
  - Modify the "user definable" properties of the response to the GET request.
  - Make a `PUT /endpoint/{id}` request, passing in the complete modified object.




### Successful `DELETE` requests


- Will now return a `204 No Content` response when successful.  No response body will be returned


## Endpoint Specific Changes

### The User Object
- The `user_name` property has been renamed to `name`.
- The `user_email` property has been renamed to `email`
- The `user_id` property has been renamed to `id`
- A new `debits_as_negative` property has been added. This describes how how to interpret negative or positive amounts on any transactions.

[View v1/v2 differences](./changelog-visual#user-object-row)

[View v2 User Object Docs](../v2/#models/userObject)


### The Category Object
- The `children` property, which is present only when the `is_group` property is `true`, is no longer nullable.  
  - The `children` property will always be the list of categories that belong to the group.
  - The Category Objects in the `children` property of a Category Group will always have value of `false` for `is_group` and will never have a `children` property.
  - Category Groups with no child categories will have a `children` property with an empty list, ie: `[]`, and not null.
- The `group_category_name` property which only existed under certain circumstances in the V1 API has been removed.

[View v1/v2 differences](./changelog-visual#category-object-row)

[View v2 Category Object Docs](../v2/#/model/categoryobject)


### categories endpoints


- **`GET /categories`** changes: [View Docs](../v2/#tag/categories/GET/categories)
  - The default response to this endpoint (with no query params) will be an alphabetized nested category list, where grouped categories are listed in the `children` property of the Category Group.
    - The length of the default response (or the response when `format` is set to `nested`) is **NOT** the total number of categories and category groups.  It is the number of category groups and ungrouped categories.
  - When the optional `format` property is set to `flattened`, the response will be a list that includes all categories and category groups.  Category groups will have a `children` property that is a list of category objects that belong to that category.     
    - Grouped categories are represented twice in a `flattened` response.
    - The length of the `flattened` response is the total number of category and category groups.
  - A new query param `is_group` is supported.   When set to `false` the response includes the complete list of categories without any category groups.
    - This may be useful for generating a list of categories that are "assignable" to a transactions.
    - If `is_group` is set to `true`, the response is a list of category groups, with fully populated `children` properties.  Ungrouped categories are not returned.
    - When `is_group` is set, the `format` parameter is ignored. 
  - The v1 endpoint only included the `category_group_name` (which is now renamed `group_name`) property for grouped categories in response when the `flattened` query parameter was set to `nested`
    - The `group_name` attribute is present only on categories that belong to a group
    - `group_name` will be present in all returned categories that belong to a group across all APIs and query parameter combinations
- **`GET /categories/{id}`** changes: [View Docs](../v2#tag/categories/GET/categories/{id})
  - The v1 endpoint did not include the `created_at` and `updated_at` properties in the returned category object.
    - The category object will now have these properties and be consistent across all APIs.
- **`POST /categories/{id}`** changes: [View Docs](../v2/#tag/categories/POST/categories)
  - This single endpoint is now used to create either a new category or a category_group
    - When creating a category group  the `is_group` property must be set to `true`
      - Optionally, the `children` property can be set to an array of child categories.  
        - The `children` property is an array which may be populated with just the ids, or the complete complete category objects for existing categories.  As long as these ids or objects exist and are not category groups they will become children of the created object.
        - The `children` array may also include strings which will be used as the names of new categories that will be created as children of the new category group.
        - The `children` array may include a mix of ids, category objects, and strings.
    - When creating a category, you may set the `group_id` property to the id of an existing category group and the newly created category will belong to that group.
  - This endpoint now returns the newly created category or category group object in the response body when successful.
    - V1 version returned a boolean true if the category was created.
    - If a category group was created, it the response will include complete category objects in the `children` array.
- **`PUT /categories/{id}`** changes: [View Docs](../v2/#tag/categories/PUT/categories/{id})
  - This single endpoint can be used to edit both categories and category groups,
    - It is not permitted to convert an existing category to a category group or vice versa; therefore, attempting to change the value of the `is_group` property of a category or category group will result in an error.
  - This endpoint can be used to modify the child categories associated with an existing category group.
    - To add or remove candidates from an existing category group, update the `children` property of the category group with an updated list of existing category IDs, existing category objects, or strings which will be used as the names of new child categories that will be created as part of the request.
      - If the `children` specified belong to another category group, they will be moved to the target category group.
      - The `children` specified will completely replace the existing child categories of the target category group.
  - This endpoint now returns the newly updated category object in the response body when successful.
    - V1 version returned a boolean true if the category was updated.
- **`POST /categories/group/{group_id}/add`** has been removed.
  - To create a new category group call [POST /categories](../v2/#tag/categories/POST/categories) passing in a request body with the `is_group` property set to true.
- **`POST /categories/group/:group_id/add`** has been removed.
  - To modify the children of an existing category group, call the [PUT /categories/{id}](../v2/#tag/categories/PUT/categories/{id}) endpoint with an updated `children` property in the request body.
    - It is now possible to modify other attributes of a category group, such as `name` and `description` using this endpoint as well.
- **`DELETE /categories/{id}`**  changes: [View Docs](../v2/#tag/categories/DELETE/categories/{id})
  - This endpoint now returns a 404 if the requested category id does not exist.
  - This endpoint now returns a 204 upon a successful delete.  No response body is provided.
  - This endpoint now supports an optional query parameter `force`.
    - When `force` is set to `true` the category will be deleted even if if has dependencies.
  - If the `force` parameter is not set to `true` and the requested category has dependencies, a 422 "Unprocessable Content" status is returned.
    - The response body in this case will include a `category_name` property and a `dependents` property.
    - The `category_name` property, which was in the `dependents` object in the v2 response is now a top level property in the main response body.
- **`DELETE /categories/{id}/force`** has been removed.  Use the `force` query parameter on the `DELETE /categories/{id}` instead


### The Tags Object
- The following properties have been added to the tag object
  - `updated_at` - Date and time the tag was last updated (in the ISO 8601 extended format).
  - `created_at` - Date and time the tag was created (in the ISO 8601 extended format).
  - `archived_at` - Date and time the tag was archived (or null if not not archived).

[View v1/v2 differences](./changelog-visual#tags-object-row)

[View v2 Tag Object Docs](../v2/#models/tagObject)


### tags endpoints

- **`GET /tags`** changes:  [View Docs](../v2/#tag/tags/GET/tags)
  - The list of tags returned in now in alphabetical order to match the list seen in the GUI
- **`GET /tags/{id}`** is added: [View Docs](../v2/#tag/tags/GET/tags/{id})
- **`POST /tags`** is added: [View Docs](../v2/#tag/tags/POST/tags)
- **`PUT /tags/{id}`** is added: [View Docs](../v2/#tag/tags/PUT/tags/{id})
- **`DELETE /tags/{id}`** is added: [View Docs](../v2/#tag/tags/DELETE/tags/{id})

### The Transaction Object

The transaction object has changed significantly in v2. A transaction has properties that reference other objects in Lunch Money such as categories, accounts, and tags. In v1 the details for these related object were "hydrated".  In the context of RESTful APIs, "hydration" generally refers to populating an object with all its data. In the Lunch Money APIs, hydration generally relates to providing the details of associated objects that are also referred to by their IDs in the response body.


To optimize the responsiveness of the v2 Lunch Money APIs, the transaction object will no longer be hydrated. Categories, accounts, and tags are now returned as IDs only.  Details of these objects can be retrieved by calling the appropriate endpoint using the supplied ID.   Developers are encouraged to maintain a local cache of these objects to reduce the number of API calls.


Specific changes are:
- The `amount` continues to be a string that represents the amount of the transaction in whatever currency is specified in the `currency` property of the transaction.
  - In the v1 API, the optional `debits_as_negative` query param controlled how to interpret the amount.  If this parameter was set to false (the default), a negative `amount` value would indicate a credit transaction.
  - In the v2 API, this parameter is no longer supported and the value in the `amount` field is set to negative or positive based on the user's `debits_as_negative` property. This setting is true by default
- The `payee`, `category_id`, and `notes` fields in the returned transaction objects now match the values seen on the transactions page in the GUI.
  - If these values have been changed due to a recurring rule a new `overrides` object will provide info on what the previous values for these fields were.
  - The `display_name` and `display_notes` fields have been removed.
  - Details of the recurring_item that caused the overrides can be found via the `GET /recurring/{id}` method and passing the value from the transaction's `recurring_id` property 
- Other than `category_id`, no other category related fields are returned.   This includes all the `category_*` fields in the v1 object as well as the `is_income`, `exclude_from_budget`, and `exclude_from_totals` properties which are derived from the category.
- The `status` property now has only three supported values, `reviewed`, `unreviewed`, and `deleted_pending`.
  - Transactions whose `is_pending` property is true will always have a `status` of `unreviewed`. 
- The `has_children` property has been removed.
- A new `is_parent` property has been added.  It is set to `true` for transactions that been split.
- The `children` property is no longer present on transactions unless they are the parent of a split transaction or a transaction group created by grouping multiple transactions together.
- By default, the parents of transactions that have been split are no longer returned by the `GET /transactions` endpoint.
  - Parents of a split transaction have the property `is_parent` set to `true`
  - If a parent transaction is queried directly via the `GET /transactions/{id}`, it will include a `children` property which is an array of of the objects for the split transactions.
  - The ID of parent transactions can be found in the `parent_id` property of split transactions which are returned by default.
- By default, transactions that have been grouped are no longer returned by the `GET /transactions` endpoint
  - Grouped transactions have the property `is_group` set to `true`.
  - Details of the transactions that make up the group can be queried by passing the ID of a grouped transaction to the `GET /transactions/group/{id}` endpoint.
 - A new property `manual_account_id` replaces the `asset_id` property. The other v1 Transaction Object properties related to manual accounts, which were named `asset_*`, are no longer included and can be found by calling `GET /manual_account/{id}` endpoint using the value returned in the `manual_account_id` property.
 - Except for the `plaid_account_id` property, the other v1 Transaction Object properties related to plaid accounts, which were named `plaid_*`, are no longer included and can be found by calling `GET /plaid_account/{id}` endpoint using the value returned in the `plaid_account_id` property.
 - The `plaid_metadata` property is no longer returned by default as part of the v2 Transaction Object. This can be added by explicitly setting the new query parameter, `include_metadata` to `true` on the `GET /transactions/{id}` API.
 - The `tags` property has been replaced by a `tag_ids` property which is a list of tag IDs associated with the transaction.  If no tags are associated with the transaction this will be an empty list.  The other elements of the tag object which were included in the v1 Transaction Object are no longer returned and can be found by calling the `GET /tags` endpoint.
 - A new property `custom_metadata` has been added to the Transaction Object.  This can be added and updated in transactions that are created or modified via the API. 
 - A new property `files` has been added to the Transaction Object.  It is an array of objects that describe any files that have been attached to the transaction.

 
[View v1/v2 differences](./changelog-visual#transaction-object-row)

[View v2 Transaction Object Docs](../v2/#models/transactionObject)



 ### transactions endpoints

 The transactions endpoint is one of the most complex in Lunch Money.  To simplify the documentation, it is broken down into four subsections:

 #### Endpoints that return or operate on a single transaction

- **`GET/transactions/{id}`** has the following changes [View Docs](../v2/#tag/transactions/GET/transactions/{id})
  - This endpoint returns all the details associated with the requested object, including properties that are not returned by the bulk `GET /transactions` endpoint.  These include:
    - `plaid_metadata` 
    - `custom_metadata`
    - `files`
    - `children` - if the returned transaction has either the `is_parent` or `is_group` property set to `true`
- **`PUT /transactions/{id}`** has the following changes [View Docs](../v2/#tag/transactions/PUT/transactions/{id})
  - This endpoint now expects a request body that specifies the properties of the transaction to update.  Any property the belongs to the transaction object may be passed in the request body, but "system set" properties such as "id" or "updated_at" will be ignored.
  - This endpoint no longer accepts the split parameter. Use [POST /transactions/split/{id}](../v2/#tag/transactions-split/POST/transactions/split/{id}) to split transactions
  - This endpoint no longer accepts the `debit_as_negative` property in the request body. To determine how to interpret positive and negative amounts inspect the `debits_as_negative` property associated with the user object returned from a [`GET /me](../v2/#tag/me/GET/me) request.
  - This endpoint no longer accepts the `skip_update_balance` property. Set the optional `update_balance` query parameter to `false` to skip the account balance update.
- A new **`DELETE /transactions/{id}`** has been added [View Docs](../v2/#tag/transactions/DELETE/transactions/{id})
  - It is now possible to delete a transaction by passing it's ID to this endpoint.
  - Grouped or split transactions must be ungrouped or unsplit before they can be deleted.
  - This API returns a `204 No Content` response when successful.

 #### Endpoints that return or operate on an array of transactions

- **`GET /transactions`** has the following changes [View Docs](../v2/#tag/transactions-bulk/GET/transactions)
  - To improve responsiveness, the following properties are not included in the returned Transation Objects by default:
    - `plaid_metadata` - enable via the `include_metadata` query parameter
    - `custom_metadata` - enable via the `include_metadata` query parameter
    - `children` - enable via the `include_children` query parameter
    - `files` - enable via the `include_files` query parameter
  - Parents of split transaction are no longer returned by default. This matches the behavior of the transactions page in the Lunch Money GUI.
    - A query parameter `include_split_parents` can be set to true to override this behavior.
  - Transactions that have been grouped are no longer returned. Only the created transaction group is returned by default.  Transactions that were grouped can be obtained by passing the ID of a transaction with the `is_group` property set to `true` to the `GET /transactions/group/{id}` endpoint.  
  - The v1 query parameter `pending` has been renamed `include_pending` and can be set to "true" to have pending transactions included in the response.
    - As with the v1 `GET /transactions` endpoint the v2 endpoint does not return pending transactions by default.  
- **`POST /transactions`** has the following changes [View Docs](../v2/#tag/transactions-bulk/POST/transactions)
  - The following properties have been renamed in the Create Transaction Object
    - `asset_id` is now `manual_account_id`
    - `tags` is now `tag_ids`
  - It is now permissible to set a `plaid_account_id` property on the Create Transaction Object
    - This will generate an error if the account does not exist, or does not have the "Allow Modifications To Transactions" property toggled on.
    - If this property is set the `manual_account_id` property must not also be set on the same transaction.
  - It is no longer possible to create a new tag "on the fly" by setting a new tag name on a transaction object. To insert a transaction that has a new tag, first call the `POST /tags` API to create the tag, and then add the new tag's ID to the `tag_ids` property in the new transaction in the request body.
  - The `debit_as_negative` property in the request body is no longer supported. Developers should examine the value of the `debit_as_negative` property of the user object to determine what negative and positive amounts represent and perform any transformations, as needed, on the client side.
  - The behavior for a request that includes transactions with duplicate `external_id` fields has changed
    - If duplicate `external_ids` are found on multiple transactions with the same `manual_account_id` within the request body, this is treated as an error.  A 400 will be returned with an error response indicating which transactions were duplicates.
    - If `external_id`s in the request body conflict with `external_id`s on existing transactions with the same `manual_account_id`, data on these will be returned in a 201 response, but any non duplicate transactions in the request will still be inserted.
  - A 400 response is now returned if the request body has an empty `transactions` array.
- A new **`PUT /transactions`** has been added [View Docs](../v2/#tag/transactions-bulk/PUT/transactions)
  - It is now possible to modify the properties of multiple transactions with a single API call
- A new **`DELETE /transactions`** has been added [View Docs](../v2/#tag/transactions-bulk/DELETE/transactions)
  - It is also now possible to submit a list of transaction `ids` to be deleted to this bulk delete endpoint.
  - As with the single transaction DELETE endpoint, grouped or split transactions must be ungrouped or unsplit before they can be deleted.
  - This API returns a `204 No Content` response when successful.
  - If any of the provided transaction ids are invalid and array of errors will be returned.
  

 #### Endpoints for grouping or ungrouping transactions

- **`POST /transactions/group/`** has the following changes [View Docs](../v2/#tag/transactions-group/POST/transactions/group)
  - The `transactions` property on the request body has been renamed to `ids`.  This is still a list of existing transaction IDs to delete.
  - The `tags` property on the request body has been renamed to `tag_ids`.  This is still a list of existing tag IOs to add to the new grouped transaction.
  - If no `category_id` property is supplied, and all the transactions in the group have the same category, the grouped transaction will inherit that category.  
  - It is now possible add a `status` property to the request body.  This can be set to `unreviewed` to leave the grouped transaction in that state.
- **`DELETE /transactions/group/{group_id}`** has the following changes [View Docs](../v2/#tag/transactions-group/DELETE/transactions/group/{id})
  - This API now returns a 204 No Content on success.
  - The transaction group is deleted and the transactions that made up the group are restored to "normal" transactions, as in the v1 API.

 #### Endpoints for splitting or unsplitting transactions

- **`POST /transactions/split/{id}`** endpoint has been added [View Docs](../v2/#tag/transactions-split/POST/transactions/split/{id})
  - Use this instead of `PUT /transactions` to split a transaction into smaller transactions.
  - This will now fail if you attempt to split an existing transaction that has already been split.  The v1 behavior was to delete the existing children and create new ones.
  - Upon success this function will return the updated transaction with a `is_parent` property set to true and a fully populated `children` array property.
- **`DELETE /transactions/split/{parent_id}`** endpoint has been added [View Docs](../v2/#tag/transactions-split/DELETE/transactions/split/{id})
  - Pass the id of a split transaction's `parent_id` property to unsplit a transaction. 
  - This replaces the v1 `POST /transactions/unsplit` endpoint

#### The Transaction Attachment Object

The `transactionAttachmentObject` is a new object type that represents a file attached to a transaction. It has the following properties:
- `id` (integer) - The unique identifier for the attachment
- `uploaded_by` (integer) - The ID of the user who uploaded the file
- `name` (string) - The original filename of the uploaded file
- `type` (string) - The MIME type of the file
- `size` (integer) - The size of the file in bytes
- `notes` (string, optional) - Any notes associated with the attachment
- `created_at` (string) - The date and time when the file was uploaded (in ISO 8601 format)

#### Endpoints for managing file attachments

- **`POST /transactions/{transaction_id}/attachments`** [View Docs](../v2/#tag/transactions/POST/transactions/{transaction_id}/attachments)
  - This endpoint allows you to attach a file to a transaction
  - The file is uploaded as a multipart/form-data request
  - Optional notes can be included with the file
  - Returns the created file attachment object
- **`GET /transactions/attachments/{file_id}`** [View Docs](../v2/#tag/transactions/GET/transactions/attachments/{file_id})
  - Returns details about a file attachment, including the transaction it's attached to
  - The response includes the file metadata and the ID of the transaction it belongs to
- **`GET /transactions/attachments/{file_id}/url`** [View Docs](../v2/#tag/transactions/GET/transactions/attachments/{file_id}/url)
  - Returns a pre-signed URL that can be used to download the file
  - The URL is temporary and should be used immediately
- **`DELETE /transactions/attachments/{file_id}`** [View Docs](../v2/#tag/transactions/DELETE/transactions/attachments/{file_id})
  - Removes a file attachment from its transaction
  - Returns a 204 No Content response on success

### The Recurring Object 

The recurring object has been significantly refactored from the recurring items object in v1.  While there haven't been major changes to the properties that make up the object, they have been reorganized to provide greater clarity to their meaning.

Essentially the object's properties are organized into one of four categories:
  - Top Level properties are attributes of the recurring items itself such as `id`, `description` and `source`.
  - The `transaction_criteria` object contains the properties that are used to determine if a transaction matches the recurring item, such as `payee`, `amount` and an optional account id.
  - The `overrides` object contains properties that will be updated in matching transactions such as `payee`, `notes` and `category_id`
  - The `matches` object contains details about the dates that matching transactions are expected, the transaction_ids for transactions that are found, and the dates when transactions are expected, but have not been found

Specific changes are:
  - A `status` property has been added.  Recurring items with a status of `suggested` do not modify transactions, while recurring items with a status of `reviewed` do.
  - The `start_date` and `end_date` properties now belong to the `transaction_criteria` object.  These represent the date range that a transaction must fall into to match.
  - The `granularity` and `quantity` properties now belong to the `transaction_criteria` object.  These define how frequently recurring transactions are expected to occur.
  - The `billing_date` property has been renamed to `anchor_date` and moved to the `transaction_criteria` object.  This date is used in conjunction with the `granularity` and `quantity` properties to identify matching transactions.
  - The `original_name` property has been renamed to `payee` and is now part of the `transaction_criteria`.
  - The `amount`, `currency`, `to_base` and `plaid_account_id` properties are now all part of the `transaction_criteria` object.  The `asset_id` property has been renamed to `manual_account_id` and is also part of `transaction_criteria`.
  - The `payee`, `notes` and `category_id` properties are now part of the `overrides` object.  When present, they represent the values that will be displayed in matching transactions.
  - The `category_group_id`, `is_income`, and `exclude_from_totals` properties have been removed.  These are attributes of the category which can be discovered by examining the category with `overrides.category_id`.
  - The `date` property which was populated with the date that the request was run, has been replaced by `request_start_date` and `request_end_date` in the `matches` object.  These specify the range that was set in the request and was used to populate the dates and transactions in the rest of the `matches` object.
  - The `occurrences` property has been renamed to `expected_occurrence_dates` and is now part of the `matches` object.  It contains an array of dates within the request range where recurring transactions are expected.  In v1 the `occurrences` property would include the last expected date prior to the range, and the next expected date after the range.  In v1, the dates are limited by the range.
  - The `transactions_within_range` property has been renamed to `found_transactions` and is now part of the `matches` object.  It is now populated by an array of objects that include a `date` and a `transaction_id` property which describe the found matching transactions in the range.  It is now populated solely by transactions within the request range and will not include the last transaction prior to the range as it did in v1.
  - The `missing_dates_within_range` has been renamed to `missing_transaction_dates` and is now part of the `matches` object.  It is populated by the subset of dates in the `expected_occurrence_dates` where no matching transaction was found.


[View v1/v2 differences](./changelog-visual#recurring-object-row)

[View v2 Recurring Object Docs](../v2/#models/recurringObject)


 ### recurring endpoints

The endpoint to access Recurring Items has been renamed from `/recurring_items` to `/recurring` 

- **`GET /recurring`** has the following changes [View Docs](../v2/#tag/recurring/GET/recurring)
  - Now supports an `end_date` as well as a `start_date` query parameter.
    - If neither is set, the current month is used as the range for populating the `matches` object for each recurring object in the response.
    - Ranges can now be shorter or longer than one month by using both the `start_date` and `end_date` query parameters.
    - When used, both parameters must be set.  It is no longer valid to specify only a `start_date`.
  - Now supports an optional `include_suggested` query parameter.
    - The v1 API only returns reviewed recurring items, and that is the default behavior in v2.
    - Recurring with a `status` of `suggested` can be added to the response by setting this parameter to `true`.
- **`GET /recurring/{id}`** has been added [View Docs](../v2/#tag/recurring/GET/recurring/{id})
  - This endpoint is now available to get a single recurring object.
  - It supports the optional `start_date` and `end_date` query parameters as described above.

### The Manual Account Object (formerly Asset Object)

The term `asset` which was used for an endpoint and an object that represented manually created accounts has been changed to simply `manual_account`.

The properties of this object are the same as those for the v1 `asset` with the following exceptions:
  - `type_name` has been renamed `type`.
    - The v1 API sometimes returned accounts with a `type_name` of `depository`.   Accounts that had this `type_name` will now return a `type` with the value of `cash`.
  - `subtype_name` has been renamed `subtype`.
  - `exclude_transactions` has been renamed `exclude_from_transactions`.
  - `updated_at` property has been added.
  - `external_id` property has been added. This can be added and updated only via the API.
  - `custom_metadata` property has been added. This may be any valid json object less than 4mb and can be added and updated only via the API.
    
[View v1/v2 differences](./changelog-visual#manual-account-object-row)

[View v2 Manual Account Object Docs](../v2/#models/manualAccountObject)

### manual_accounts endpoints

- **`GET /manual_accounts`** replaces v1 `GET /assets` [View Docs](../v2/#tag/manual_accounts/GET/manual_accounts)
- **`GET /manual_accounts/{id}`** has been added [View Docs](../v2/#tag/manual_accounts/GET/manual_accounts/{id})
- **`POST /manual_accounts`** replaces v1 `POST /assets` [View Docs](../v2/#tag/manual_accounts/POST/manual_accounts)
- **`PUT /manual_accounts`** replaces v1 `PUT /assets` [View Docs](../v2/#tag/manual_accounts/PUT/manual_accounts/{id})
- **`DELETE /manual_accounts`** has been added [View Docs](../v2/#tag/manual_accounts/DELETE/manual_accounts/{id})


### The Plaid Account Object

The properties of the Plaid Account Object are unchanged from the v1 version of the API with the following exception:
  - `allow_transaction_modification` property has been added.  This represents the state of the "Allow Modification To Transactions" toggle seen in the Plaid Accounts Detail view.   This is enabled by default.  When disabled, attempts to insert new transactions associated with this account will fail.

[View v1/v2 differences](./changelog-visual#plaid-account-object-row)

[View v2 Plaid Account Object Docs](../v2/#models/plaidAccountObject)


### plaid_accounts endpoints

- **`GET /plaid_accounts/{id}`** has been added [View Docs](../v2/#tag/plaid_accounts/GET/plaid_accounts/{id})
-  **`POST /plaid_accounts/fetch`** endpoint has the following changes [View Docs](../v2/#tag/plaid_accounts/POST/plaid_accounts/fetch)
  -  The response body now includes the list of Plaid Accounts that a fetch was triggered for.
 