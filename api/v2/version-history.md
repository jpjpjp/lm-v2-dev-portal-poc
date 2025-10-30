# Version History

The Lunch Money API spec uses a modified version of SEMVER for its versioning methodology as follows.
- The major version is the API version.  This will always be 2 for the v2 version of the API.
- The minor version represents the number of main endpoints the current version of the spec supports.  For example, a version of the API that supports the /me, /categories, and /transactions endpoints would have a minor version of 3.
- The revision number represents the number of updates since the last endpoint was added.  For example, each time changes are made to one of the existing three APIs as described above, the revision number will be bumped.

## v2.8.0
- A new v2/summary endpoint has been added. This replaces the v1/budgets endpoint, which no longer works with the new budgeting feature.
- The 'pending' value for `status` in the transactions object has been deprecated.  This was redundant with the `is_pending` boolean property.
  - A new `is_pending` query parameter has been added to the GET /transactions endpoint.
- POST /transactions/split/{id} and POST /transactions/group now return a single transaction with children and not an array of transactions.
- A `custom_metadata` property has been added to the Manual Account object.
- The maximum length of the Manual Account's `subtype` property has increased from 75 to 100 characters.
- An empty `description` string may now be passed in the request body for the POST and PUT /categories endpoint

## v2.7.11
- Added new endpoints for managing transaction file attachments:
  - POST /transactions/{transaction_id}/attachments - Upload a file to be attached to a transaction
  - GET /transactions/attachments/{file_id} - Get a signed URL to download the file attachment
  - DELETE /transactions/attachments/{file_id} - Remove a file attachment from a transaction
- Added support for file attachments in transaction objects:
  - The `files` property is now included in transaction objects when `include_files` is set to true
  - The `files` property contains an array of `transactionAttachmentObject` objects
  - File metadata (type and name) is properly returned when downloading files
- The v2 /recurring endpoint has been renamed back to /recurring_items.
- The `overrides` property has been removed from the transaction object. This ws meant to provide the original info that was overridden by a recurring rule, but turned out to be un-implementable.
- The bulk PUT /transactions API has been completely redesigned.  It now is similar to the POST /transactions API, taking an array of transactions which must include an id along with any other properties to do be updated.
- The payee/date/amount transaction duplication logic that is triggered by the `skip_duplicates` request body property on a POST /transactions request will now be applied to plaid accounts as well as manual accounts.
- Improved readability of Request validation errors. The example responses in the spec and those returned by the mock server have been updated to match those that will be returned by the actual implementation of the v2 API. 
- The v2 proxy using v1 service has been removed from the spec.


## v2.7.10
- Add a transactionAttachments type that captures details about files attached to a transaction.
- Modified the behavior of the GET /transactions endpoint:
  - For performance reasons, transactions returned by this endpoint will, by default, not include `plaid_metadata`, `custom_metadata`, `children`, or `files` properties.  These properties ARE provided by default in the GET /transactions/:id endpoint.
  - A new `include_children` query parameter has been added.  If set to `true`, transaction groups will include a `children` property with an array of transactions that make up the group.
  - A new `include_metadata` query parameter has been added.  If set to `true`, transactions returned will include the properties `plaid_metadata` and `custom_metadata`, which will be `null` when metadata is not associated with the transaction.
  - An `include_files` query parameter is added.  When set to `true`, a `files` property is returned with an array of objects that describe any attachments to the transaction.
  - For completeness, an `include_split_parents` query parameter is added.  Will override default behavior and return transactions that were split when set.  The split transactions are also returned.  When used in conjunction with the `include_children` parameter, split parents will have a `children` property that also includes the split transactions.
  - Documented that `include_pending` query param is ignored if `status` query param is also set.
- Modified the behavior of the GET /transactions/:id endpoint:
  - A successful response will always include all available transaction details, including `plaid_metadata`, `custom_metadata`, and `files` properties.
  - When `is_group` or `is_parent` is true in the response, it will also include a `children` property.
- Modified the response body returned from a successful POST /transactions request:
  - A `skipped_duplicates` array property will always be returned along with the `transactions` array.
  - This will include details on any requested transactions that were not inserted due to duplicate detection.
  - Duplicates will always be flagged if the `manual_account_id` and `external_id` of a requested transaction match existing transactions.
    - Duplicates may also be flagged if the `skip_duplicates` request body property was set to `true`, and the requested transaction has the same `manual_account_id`, `payee`, `date`, and `amount` of an existing transaction.
  - The insertTransactionResponseObject is now included in the models section of the documentation.
- Updated the examples for the PUT /transactions (bulk) endpoint
- Increased the max length of the `subtype` string that can be set on a manual account object to 75 characters
  
## v2.7.9
- Updated several type definitions in the spec to use the type `integer` instead of `number` for properties such as ids, orders, and indexes which are always an integer.   Amounts, balances and limits still use type `number`.

## v2.7.8
- The request body for a PUT /tags request will now accept, and ignore, system set tag object properties such as `id` and `created_at`.
- Response codes for the POST /plaid_accounts/fetch endpoint have changed:
  - A successful fetch request will return a 202 ACCEPTED status with no response body.
  - A fetch request made within one minute of a previous fetch request will return a 425 TOO EARLY response.
- Setting the query param `category_id` to 0 on a GET /transactions request will now return un-categorized transactions.
- The `to_base` property of the `transaction_criteria` object in the recurringObject returned by the `GET /recurring` API has been removed. This property as this is not used to match transactions to a recurring item.
  
## v2.7.7
- The ability to set the `order` property of a category via the API has been removed.
  - Setting `order` in the body of a POST /categories request will result in a validation error.
  - The value of `order` in the body of PUT /categories request is treated as a "system defined" property and is ignored.
- This release makes some non-functional changes to the spec to make it behave better with programmatic consumers (ie: type and sdk generators).
  - The transactionObject's `children` array now has a defined `items` type.
  - There is no default value for the `children` property in the request body schema for a POST or PUT /categories request.  If this property is not explicitly set it is treated as if it does not exist.
  - The GET /categories optional `is_group` query parameter is now defined a boolean instead of an enum of true and false
- The Models section of the documentation has been slightly changed as follows
  - We are now consistently showing only the primary Object for each endpoint.  Schemas for request bodies are not shown in this section but remain in the individual endpoint's documentation.
  - Renamed the `AccountTypeEnum` to `accountTypeEnum` to maintain consistency in schema naming style

## v2.7.6
- This release adds the `to_base` property to the Manual and Plaid Account objects to align with a recent change in the v1 API.

## v2.7.5
- This release adds the following new transactions endpoints:
  - PUT /transactions/:id
    - This works similarly to the same endpoint in v1.
  - PUT /transactions
    - This new endpoint enables updating multiple existing transactions with a common update object.
- This release modifies the following endpoint behavior
  - POST/PUT /categories
    - It is now permissible to specify the ids of categories that belong to another category group in the `children` property of a request. The categories will be moved to the group being created or updated without error.
    - It is also now permissible to include strings in the `children` property.  These will be used as the names of new child categories that will be created.
- This release also updates the following schemas:
  - Correctly specifies that all properties of the manualAccountObject are required
  - Updates the updateManualAccountRequestObject and updatePlaidManualAccountRequest object to allow the `balance` property to be either a string or a number.
  - Updates the userObject to include the `debits_as_negative` property.  The documentation for Transaction Objects returned by GET requests have been updated to reflect how/if this setting affects the `amount` property of the transaction.
  - Updates the childCategoryObject to restore the `exclude_from_budget`, `exclude_from_totals` and `is_income` properties.  These properties are inherited from the Category Group and not settable but are provided for convenience.

## v2.7.4
- This release adds the following new transactions endpoints:
  - POST /transactions/group/
    - This works similarly to the same endpoint in v1. Use this endpoint to group a set of transactions into a single transaction.
  - DELETE /transactions/group/{id}
    - This endpoint replaces the v1 /transactions/ungroup endpoint.
    - Use this endpoint to delete a transaction group and restore, the grouped transactions to their "normal" transaction state.
  - DELETE /transactions/split
    - This endpoint replaces the v1 /transactions/unsplit endpoint and the v2 version that was introduced in the previous release.
    - Use this endpoint to delete the split transaction and restore the parent transaction to the "normal" transaction state.
- The documentation for the various /transactions endpoints has been reorganized:
  - The `transactions` section covers endpoints that impact a single transaction.
  - The `transactions (bulk)` section covers endpoints that impact multiple transactions.
  - The `transactions (group)` section covers endpoints related to grouping and ungrouping transactions.
  - The `transactions (split)` section covers endpoints related to splitting and unsplitting transactions.

## v2.7.3
- This release adds the following new transactions endpoints:
  - POST /transactions/split/{id}
    - This is a new endpoint in v2. Use this endpoint to a split a transaction into multiple child transactions.  The POST /transactions endpoint will no longer support splits
  - POST /transactions/unsplit
    - This has only minor changes from the existing v1 version of this API
  - DELETE /transactions/{id}
    - This new endpoint will delete a single transaction by ID
  - DELETE /transactions
    - This new endpoint will bulk delete all the transactions who's IDs were submitted in the `ids` property in the request body.
    - Both DELETE endpoints will generate errors if the IDs submitted belong to group or split transactions or do not exist.
- Changes to existing endpoints:
  - It is now permissible to set a `plaid_account_id` on a transaction passed in to POST /transactions.
- The following objects have been modified:
  - The Plaid Account Object 
    - A new boolean `allow_transaction_modifications` property has been added.  This represents the state of the "Allow Modifications To Transactions" toggle which is enabled by default.   When this property is false, attempts to add transactions to this account will fail.


## v2.7.2
- The following objects have been modified:
  - The Transactions Object
    - An optional `custom_metadata` object has been added.  This can be set or modified via the API.
- This release adds the following to the /transactions endpoint
  - POST /transactions
    - Properties in the transaction object passed in the request have been updated to new v2 naming standards
      - A new `custom_metadata` may be included as a property on the new transaction objects.   This can be any valid JSON object.
    - The `debit_as_negative` property on the request body is no longer supported
    - Requests that contain transactions with `external_ids` behave differently
      - Duplicate `external_ids` within the request body are treated as an error
      - Requested transactions with an `external_id` that already exists in the database are now skipped.  Any remaining transactions in the request are inserted.
- The following endpoints have been modified:
  - GET /transactions/{id}
    - The `include_plaid_metadata` parameter name has changed back to `include_metadata` and will now return both plaid and custom metadata.
- Tests
  - Tests now require Portman version 1.30.7 or later.
  - A new script `check-for-duplicates.sh` was added to the tests/scripts directory.  When the newman output is redirected to a file, this script can identify tests that were skipped.


## v2.7.1
- This release updates the sample responses for the /recurring endpoints

## v2.7.0
- This release adds the initial version of the /plaid_accounts endpoint
- Updated changelog to mention the renamed /manual_accounts endpoints
- Minor update to the Manual Account Object Schema
  - A new `external_id` property was added which can be set via the POST and PUT /manual_transactions endpoints
- Minor update to the `GET /categories` endpoint
  - A new query optional parameter `is_group` can be set to `false` in order to get the API to return only a set of assignable categories.

## v2.6.0
- This release adds the initial version of the /manual_accounts endpoint

## v2.5.0
- This release adds the initial version of the /recurring endpoint
- A minor update was made to the response body of a DELETE /categories requests
  - The `category_name` property has been removed from the `dependents` object and is now returned as a top-level property in the response body.
- Made the static mock server the default server
  - v1 proxy is still available in the servers drop down.
  - The links in the changelogs work with Stoplight but not Scalar, so having this as the default makes it more likely that users will be able to navigate the changelogs.

## v2.4.1
- This release makes minor changes to the existing categories and tags endpoints
  - Tags Object
    - The properties `created_at`, `updated_at`, and `archived_at` have been added
  - DELETE /tags endpoint
    - Will now return a 422 with a dependents object if the tag is used in rules or transactions
    - Will now support a `force` query param

## v2.4.0
- This release adds the initial version of the /tags endpoint

## v2.3.7
- This release makes minor changes to the existing categories and transactions endpoints
  - Categories Object
    - The `group_name` property has been removed
  - Transactions Object
    - Restores the property name `plaid_metadata`.  This anticipates a new `user_metatdata` property in a future release.
  - GET /categories endpoint
    The `is_group` query parameter has been removed
  - GET /transactions/{id} endpoint
    - Renamed `include_metadata` to `include_plaid_metadata`
    - Added `split` as a new value for the `source` enum.  It's set for split transactions.
- It also corrects typos in the names of the servers listed in the spec
- The tests in this release require a version of Portman at or above 1.30.2
  


## v2.3.6
- This release restores the hydration of children for category and transaction objects
  - Categories Object
    - The `child_ids` property of the category object is renamed `children` and is populated by fully hydrated category objects
    - The `children` property will always be present for a category group.
      - If the group has no child categories, `children` will be an empty list.
    - Objects in the `children` list property are Child Category Objects that are similar to Category Objects, but with some differences:
      - `is_income`, `exclude_from_budget`, and `exclude_from_totals` are not included since these are inherited from the category group and are no longer properties of the child categories.
      - `group_id` is never `null`, `group_name` is always present, and `children` will never be present.
  - GET /categories endpoint
    - When `format` is set to `flattened` all categories and category groups are returned in the top level list
      - Category groups will still have their `children` property
        - This is done to conform to a policy that objects should have the same properties consistently across requests
      - Since categories that belong to category groups are represented in the `children` property of their category group and also in the top level these categories can be found twice in the `flattened` response.
      - Top-level categories that happen to belong to a category group are still full Category Objects and not Child Category Objects.
        - This means they will have the `is_income`, `exclude_from_budget` and `exclude_from_totals`, properties.
    - The `format` query param now has a default value of `nested`, which means that a nested list of category objects is returned by default.
      - This approach is preferred since the flattened list has redundant info with each grouped category represented once in the `children` attribute of its category croup and then once in its own spot in the top-level list
    - An `is_group` query parameter has been added to the `GET /categories` method.  
      - This can be set to `false` to return a list of assignable categories. No Category Groups are returned.
      - Setting it to `true` will return a list of Category Groups, with fully hydrated `children` objects.  No ungrouped categories are returned.
      - When this query param is set, the value of the `format` query param is ignored if also set.
  - POST and PUT /categories endpoints
    - The request body for the `POST` and `PUT /categories` may now include a `children` property.
      - This property is a list which may contain category objects, child category objects, or simply ids.
      - Mixing the types of allowed elements in the `children` property of the request body is permitted.
    - A successful response body will always include the fully hydrated Child Category Objects in the `children` property.

## v2.3.5
- This branch adds a second demonstration server to the spec.  The first server is a traditional "mock data" server that uses static canned data.   The new server implements the V2 API on top of the V1 API and can be used to manipulate actual data in Lunch Money.   This initial version supports only the /categories endpoint.  Requests to other endpoints continue to use canned data
- Spec Updates
  - The spec now includes two servers in the servers section
  - The `synced_metadata` property of the transaction object was renamed `metadata` in anticipation of metadata availability in imported transactions associated with manual accounts.
- Test Updates
  - A large number of new tests were added in the tests/configs/variations/category-variations.yaml.  With a real backend, we can do more testing of the POST, PUT, and DELETE endpoints.   Many of these tests are skipped if the postman variable isMockServer is set to true.  (See [Test README](./tests/README.md))
  - Tests work with Portman version 1.30.0. This release addresses reported bugs and supports tests that set form encoded query params like the `ids` param on the `GET /categories` endpoint.
- Commit Hook Updates
  - The commit hook that toggles URLs in index.html and the spec between the local and deploy URLs was updated to support two separate URLs


## v2.3.4
- This branch removes hydration options from the /categories and /transaction endpoints and objects.
- Renamed the `children` property of the category object to `children` to match the new agreed upon standard for naming types and id lists.
- Eliminated the two types of category objects, opting instead for a simpler single category object.   This eliminates some of the automatic request body validation that we had but I think simplifies the docs.    Additional tests were added to ensure that
  - Category Groups have a `child_id`  and no `group_name` property and `group_id` is null
  - Categories that are part of a group have no `child_id` property, have a non null `group_id` property and have a `group_name` property.
  - Ungrouped categories have a `group_id` with a value of `null` and do no have a `children` or `group_name` property
- Renamed transaction status values "cleared" and "uncleared" to "reviewed" and "unreviewed"
- Created a visual representation of the changes to the objects in changelog-visual.html
- Continued iteration on the recurring_items object.  
  - This version include an updated schema for the recurringItems object
  - It also proposes an alternateRecurringItems object
  
## v2.3.3

- This branch introduces the `GET /transactions/{id}` endpoint.  As of this release this is the only endpoint that supports hydration of the category, account, recurring items and tags.   Hydration is enabled via query parameter.  There is also a query parameter to request synced (plaid) metadata for the transaction.

- The branch also introduces changes to the transaction object:
  - A new `overrides` object property is added to the transaction object.  This object is populated when a recurring rule has overridden one or more properties of the transaction as displayed in the transactions page on the GUI.
  - The `payee`, `category_id`, and `notes` properties will now have values that match the transactions page in the GUI.   The `display_*` properties of the transaction object have been removed.  The original values can now be found in the new `overrides` object.
  - The order of the properties in the transaction object schema is changed to introduce the new `overrides` property before any properties that may have been overridden.   The ordering should now more closely match the transaction page on the GUI.
  - The `children` property that exists in a transaction that has been split has been renamed `children`
  - The transaction object now supports the optional properties: `synced_metadata`, `hydrated_recurring`, `hydrated_category`, `hydrated_account`, and `hydrated_tags`
  - A set of required properties has been added to the transaction object's schema definition

- Schema definitions have been added for the `manualAccountObject`, `syncedAccountObject` and `recurringItemObject` in order to support hydration

- Tests were added
  - Filter transactions by category group
  - GET /transactions/{id} tests
  - Updates to /categories tests
  - Tests work with Portman version 1.28.0. A known bug with this version of portman requires that one of the /me variation tests is commented out


## v2.3.2
This branch updates the tests to take advantage of bug fixes and features added to Portman 1.28.0
- Projects that implement the Lunch Money API server should update to this version of Portman
- The portman config now sets a global directive `variableCasing` to snakeCase.   Use of this directive in prior versions caused some tests to fail
- The tests now have a global dynamic pre-request script that is run prior to any requests that take an ID as a path parameter.  This script will "reset" the variable that the ID is set to when `PORTMAN_IS_MOCK_SERVICE=true` is set in the .env-portman file.   Implementers of a Mock API server should see further details on how to define the "canned" IDs to use in the [./tests/README.md](./tests/README.md#testing-against-a-mock-service-with-immutable-data-vs-a-real-service)
- Tests added as part of this release
  - /me
    - Validate proper error response when no Authorization header is sent
  - /transactions
    - validate setting `manual_account_ids` to zero will filter out all manual account transactions
    - validate setting `plaid_account_ids` to zero will filter out all synced account transactions
    - validate setting both to zero will return on "cash transactions"


## v2.3.1
This is the initial branch using the versioning described above
- It is the v2 version of the API - hence the major version 2.
- The API Spec currently supports 3 endpoints /me, /categories, and /transactions.  hence the minor version 3.
- This version of the spec contains minor modifications to the previously published spec and therefore has a revision number 1

