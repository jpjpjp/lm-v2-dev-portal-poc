# Migrating from API v1 to v2

The Lunch Money API v2 introduces several important improvements to make the API more consistent, reliable, and easier to work with. This guide will help you update your application to work with v2.

## Getting Started

Before you begin, we recommend:

1. Reviewing the [v2 changelog](../v2/lunch-money-api-changelog.md) for a complete list of changes
2. Testing your migration in a development environment
3. Keeping your v1 integration working as a fallback until migration is complete

## Base URL Changes

The base URL for all API requests changes from:

```
https://dev.lunchmoney.app/v1/
```

to:

```
TBD
```

## General Changes

### Error Handling

The v1 API often returns a `200 OK` response even when the API fails and the response body contains an error message.  V1 apps may or may not have special logic to handle errors after the API returns a 2XX response.

In the v2 API, error responses are now more consistent and informative:

- **HTTP Status Codes**: Errors now return proper 4XX status codes:
  - `400 Bad Request` - Invalid parameters or request body
  - `401 Unauthorized` - Authentication problems
  - `404 Not Found` - Resource not found
  - `429 Too Many Requests` - Rate limit exceeded (includes `Retry-After` header)

- **Error Response Format**: All errors return a JSON object with:
  ```json
  {
    "message": "Overall error description",
    "errors": [
      {
        "errMsg": "Specific error about a parameter or property"
      }
    ...
    ]
  }
  ```

You may wish to revisit any error handling portions of your existing app.  You should no longer need to examine 2XX response bodies for error messages, but you may need to add new handlers for other types of error responses.

In addition, the v2 API does full request validation and generates errors when a request has invalid query parameters or invalid properties in a request body.   The v1 API often accepted these types of requests, sometimes with strange results.  As you move your app to v2 requests, test iteratively and adjust your code as necessary to remove invalid components from  your request.

### Response Codes

The following responses indicate a succesful API request

- **GET requests** return `200 OK` when called with proper query parameters and headers.
- **POST requests** now return `201 Created` (instead of `200 OK`) with the created object in the response body.
- **PUT requests** return `200 OK` with the updated object in the response body.
- **DELETE requests** return `204 No Content` with no response body (instead of `200 OK` with `true`).

### Updating Objects
Most Lunch Money objects are now updatable via a `PUT` request to the objects endpoint. When making a `PUT` request:
  - Request bodies must contain at least one or more "user-definable" properties to change.
  - Any "user-definable" property in a PUT will be changed in the specified object.
  - Any "system-defined" property in a PUT request will be tolerated but ignored.
  - Any other unexpected property will fail validation and return a 400 response.

- Because request bodies MAY optionally contain "system-definable" properties, developers may perform `PUT` requests using the following pattern:
  - Make a `GET /endpoint/{id}` request.
  - Modify the "user definable" properties of the response to the GET request.
  - Make a `PUT /endpoint/{id}` request, passing in the complete modified object.

Unless otherwise documented, the values for complex properties, such as objects and arrays that are set in the request body, will completely replace any previous value for these properties. Therefore, if you wish to, for example, add a new tag to a transaction or a new child category to a category group, you should first query the existing object and then update the property appropriately.  

### Property Naming Conventions

Some object and property names have changed in the v2 API. The details are listed below but in general property names have been standardized:
- Object properties use simple names (e.g., `id` instead of `category_id` on category objects)
- Related object references include the object type (e.g., `category_id` on transactions)
- Timestamps end with `_at` (e.g., `created_at`, `updated_at`, `archived_at`)
- ID arrays include the type (e.g., `tag_ids` instead of `tags`)
- Child objects are in a `children` property

All `id` properties in request and response bodies are now explicitly `integer` type instead of `number` for better validation.  Most existing v2 code will not need to change

## Object and Endpoint Specific Changes

### User Object


#### Property Renames

```javascript
// v1
{
  "user_id": 123,
  "user_name": "John Doe",
  "user_email": "john@example.com"
}

// v2
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

#### New Properties

- `debits_as_negative`: A boolean indicating how to interpret transaction amounts.

**Migration Tips**: 
- Update all references to `user_id`, `user_name`, and `user_email` in your code to use the new property names.
- Keep track of the `debits_as_negative` setting for the user.  The value of this setting will inform how you specify amounts when setting account balances and inserting transactions.

### Categories

#### Endpoint Changes

#### Creating Categories

v1 had separate endpoints for categories and category groups. v2 uses a single endpoint:

```javascript
// v1 - Create category
POST /v1/categories
{ "name": "Groceries", ... }

// v1 - Create category group
POST /v1/categories/group
{ "name": "Food & Drink", "category_ids": [...], ... }

// v2 - Create category or group (single endpoint)
POST /v2/categories
{ "name": "Groceries", ... }  // Regular category
// OR
{ "name": "Food & Drink", "is_group": true, "children": [...] }  // Category group
```

#### Adding to Category Groups

v1 had a unique endpoint for adding children to an existing category group.  
v2 allows children to be modifies via the `PUT /categories` endpoint.

```javascript
// v1
POST /v1/categories/group/:group_id/add
{ "category_ids": [1, 2, 3] }

// v2
PUT /v2/categories/:id
{ "children": [1, 2, 3] }  // Replaces all children
```

#### Deleting Categories
When attempting to delete a category, both the v1 and v2 APIs will return a `dependents` object if other Lunch Money elements (ie: transactions, rules, etc) depend on the category. Both version provide a way to force delete the category when this happens but the syntax is slightly different:

```javascript
// v1 - Force delete
DELETE /v1/categories/:id/force

// v2 - Use query parameter
DELETE /v2/categories/:id?force=true
```

#### Response Changes

- `POST /categories` now returns the complete category object (not just `{ "category_id": 123 }`)
- `PUT /categories/:id` returns the complete updated category object (not just `true`)
- `DELETE /categories/:id` returns `204 No Content` (not `true`)

#### Object Property Changes

- `children` property of a category group is never `null` - it's an empty array `[]` for groups with no children
- `group_category_name` has been removed
- `group_name` is now present on all grouped categories (not just in certain responses)

##### Query Parameters

```javascript
// v1
GET /v1/categories?format=nested  // or "flattened" - default: flattened

// v2
GET /v2/categories  // Default: nested alphabetized list
GET /v2/categories?format=flattened  // All categories including duplicates
GET /v2/categories?is_group=false  // Only assignable categories (no groups)
GET /v2/categories?is_group=true   // Only category groups
```

**Migration**:
- Add the format query parameter to GET /v2/categories requests as needed, the default has changed 
- Update category and category group creation logic to use a single endpoint with `is_group` flag
- Update group management to use `PUT` with `children` array
- Handle the new query parameters for filtering categories
- Update code that checks `children === null` to check for empty array instead

### Tags

#### New Properties

The tag object now includes:
- `created_at`
- `updated_at`
- `archived_at`

#### New Endpoints

v2 adds full CRUD operations for tags:

```javascript
// Get single tag
GET /v2/tags/:id

// Create tag
POST /v2/tags
{ "name": "Work Expense", ... }

// Update tag
PUT /v2/tags/:id
{ "name": "Business Expense", ... }

// Delete tag
DELETE /v2/tags/:id
```

#### List Ordering

`GET /tags` now returns tags in alphabetical order (matching the GUI).

**Migration**: Take advantage of the new tag management endpoints if you need to create or modify tags programmatically.

### Transactions
The transaction object has changed significantly in v2. A transaction has properties that reference other objects in Lunch Money such as categories, accounts, and tags. In v1 the details for these related object were "hydrated". In the context of RESTful APIs, "hydration" generally refers to populating an object with all its data. In the Lunch Money APIs, hydration generally relates to providing the details of associated objects that are also referred to by their IDs in the response body.


To optimize the responsiveness of the v2 Lunch Money APIs, the transaction object will no longer be hydrated. Categories, accounts, and tags are now returned as IDs only.  Details of these objects can be retrieved by calling the appropriate endpoint using the supplied ID.   Developers are encouraged to maintain a local cache of these objects to reduce the number of API calls.

```javascript
// v1 - Hydrated transaction
{
  "id": 123,
  "category_id": 456,
  "category_name": "Groceries",  // ❌ Not in v2
  "category_group_id": 789,       // ❌ Not in v2
  "category_group_name": "Food",  // ❌ Not in v2
  "is_income": false,             // ❌ Not in v2
  "tags": [                       // ❌ Not in v2
    { "id": 1, "name": "Work" }
  ],
  "asset_name": "Checking",       // ❌ Not in v2
  "plaid_account_name": "...",    // ❌ Not in v2
  ...
}

// v2 - IDs only
{
  "id": 123,
  "category_id": 456,             // ✅ Still here
  "tag_ids": [1, 2],              // ✅ Array of IDs
  "manual_account_id": 789,       // ✅ Renamed from asset_id
  "plaid_account_id": 123,        // ✅ Still here
  ...
}
```

To get full details, fetch the related objects:

```javascript
// Get category details
GET /v2/categories/456

// Get account details
GET /v2/manual_accounts/789
// OR
GET /v2/plaid_accounts/123

// Get tag details
GET /v2/tags/1
```

**Migration Strategy**: 
- Maintain a local cache of categories, accounts, and tags
- Update your code to use IDs and fetch related objects separately when needed
- Consider batching requests or using the bulk endpoints to reduce API calls

#### Removed Properties

The following properties found in the hydrated v1 Transaction Object are no longer present in the v2 Transaction object

These category-related properties are no longer included (get from category object):
- `category_name`
- `category_group_id`
- `category_group_name`
- `is_income`
- `exclude_from_budget`
- `exclude_from_totals`
- `group_category_name`

These account-related properties are no longer included (get from account object):
- `asset_institution_name`
- `asset_name`
- `asset_display_name`
- `asset_status`
- `plaid_account_name`
- `plaid_account_mask`
- `institution_name`
- `plaid_account_display_name`

#### Renamed Properties
The following properties have been renamed or refactored in the v2 Transaction object

- `asset_id` → `manual_account_id` 
- `tags` (array of objects) → `tag_ids` (array of tag id integers)
- `has_children` → `is_parent`
- `display_name` → removed (use `payee` directly)
- `display_notes` → removed (use `notes` directly)

These recurring-related properties have changed:
- `recurring_payee`, `recurring_description`, etc. are now in an `overrides` object
- Check the transaction's `recurring_id` and fetch from `/v2/recurring/:id` for details

#### Transaction Amounts

```javascript
// v1
GET /v1/transactions?debit_as_negative=false  // Query parameter

// v2
GET /v2/me  // Check user.debits_as_negative property
// Amounts are always set according to user preference
```

**Migration**: 
- Remove `debit_as_negative` query parameters and check the user object to determine how to interpret amounts.

#### Status Values

```javascript
// v1
"status": "cleared" | "uncleared" | "pending" | "recurring" | "recurring_suggested"

// v2
"status": "reviewed" | "unreviewed" | "deleted_pending"
```

**Migration**: Update status checks:
- `"cleared"` → `"reviewed"`
- `"uncleared"` → `"unreviewed"`
- Transactions with `is_pending: true` always have status `"unreviewed"`
- Update any code logic that checks if `status` is pending to use `is_pending` instead

#### Split and Grouped Transactions

**Split Transactions**:
- Parent transactions (`is_parent: true`) are not returned by default in `GET /transactions`
- Split transactions include `parent_id` pointing to the parent
- Use `GET /transactions/:id` to get parent with full `children` array
- Use `include_split_parents=true` query parameter to include parents in list

**Grouped Transactions**:
- Grouped transactions are not returned by default
- Only the group transaction (`is_group: true`) is returned
- Use `GET /transactions/group/:id` to get details of grouped transactions

**Migration**: Update code that processes split or grouped transactions to use the new endpoints when needed.

#### Creating Transactions

```javascript
// v1
POST /v1/transactions
{
  "transactions": [...],
  "debit_as_negative": false,  // ❌ Removed in v2
  ...
}

// v2
POST /v2/transactions
{
  "transactions": [
    {
      "asset_id": 123,      // ❌ Renamed
      manual_account_id: 123,  // ✅ New name
      "tags": ["Work"],     // ❌ Can't create tags on the fly
      "tag_ids": [1, 2]     // ✅ Must use existing tag IDs
    }
  ]
}
```

**Important Changes**:
- `asset_id` → `manual_account_id`
- `tags` array (could include strings to create tags) → `tag_ids` array (only existing IDs)
- `plaid_account_id` can now be set when creating transactions (if account allows modifications)
- `debit_as_negative` removed - check user object instead
- Duplicate `external_id` values within the same request are now errors (400)

**Migration**: 
- Create tags first using `POST /v2/tags` before assigning them
- Update property names in transaction creation
- Handle duplicate external_id errors
- Ensure that amounts are properly signed for inserted transactions. Regardless of the value of the users `debits_as_negative` property always use positive numbers for debits and negative numbers for credits.


#### Updating Transactions

```javascript
// v1
PUT /v1/transactions/:id
{
  "transaction": { ... },
  "split": [...],              // ❌ Moved to separate endpoint
  "debit_as_negative": false,  // ❌ Removed
  "skip_balance_update": true  // ❌ Renamed
}

// v2
PUT /v2/transactions/:id?update_balance=false  // Query parameter
{
  // Just the transaction properties
}
```

**Splitting Transactions**:
```javascript
// v1
PUT /v1/transactions/:id
{ "split": [...] }

// v2
POST /v2/transactions/split/:id
{ "splits": [...] }
```

**Unsplit Transactions**:
```javascript
// v1
POST /v1/transactions/unsplit
{ "parent_ids": [...] }

// v2
DELETE /v2/transactions/split/:parent_id
```

**Migration**: 
- Move split logic to the new endpoint
- Use query parameter for balance update control
- Remove `debit_as_negative` from requests

#### Deleting Transactions

```javascript
// v1 - No delete endpoint

// v2
DELETE /v2/transactions/:id        // Single transaction
DELETE /v2/transactions            // Bulk delete
{ "ids": [1, 2, 3] }
```

**Migration**: Update code that may have been working around the lack of delete endpoints.

#### Query Parameters

```javascript
// v1
GET /v1/transactions?pending=true&debit_as_negative=false

// v2
GET /v2/transactions?include_pending=true
// debit_as_negative removed - check user object instead
// New optional parameters:
// ?include_metadata=true      // Include plaid_metadata, custom_metadata
// ?include_files=true         // Include file attachments
// ?include_children=true      // Include children for split/grouped
// ?include_split_parents=true // Include parent transactions
```

**Migration**: Update query parameter names and remove `debit_as_negative`.

#### Transaction Attachments (New)

v2 introduces file attachments:

```javascript
// Upload attachment
POST /v2/transactions/:transaction_id/attachments
// multipart/form-data with file and optional notes

// Get attachment details
GET /v2/transactions/attachments/:file_id

// Get download URL
GET /v2/transactions/attachments/:file_id/url

// Delete attachment
DELETE /v2/transactions/attachments/:file_id
```

### Recurring Items

#### Endpoint Rename

```javascript
// v1
GET /v1/recurring_items

// v2
GET /v2/recurring
```

#### Object Structure Changes

The recurring object has been significantly reorganized:

```javascript
// v1
{
  "id": 123,
  "start_date": "2024-01-01",
  "end_date": null,
  "billing_date": "2024-01-15",
  "payee": "Netflix",
  "amount": "15.99",
  "category_id": 456,
  "category_group_id": 789,    // ❌ Removed
  "is_income": false,          // ❌ Removed (get from category)
  "exclude_from_totals": false, // ❌ Removed (get from category)
  "granularity": "month",
  "quantity": 1,
  "occurrences": { ... },
  "transactions_within_range": [...],
  "missing_dates_within_range": [...],
  "date": "2024-06-04"
}

// v2 - Reorganized into logical groups
{
  "id": 123,
  "description": "Netflix subscription",
  "source": "manual",
  "status": "reviewed",  // ✅ New: "reviewed" or "suggested"
  "transaction_criteria": {
    "start_date": "2024-01-01",  // ✅ Moved here
    "end_date": null,             // ✅ Moved here
    "anchor_date": "2024-01-15",  // ✅ Renamed from billing_date
    "payee": "Netflix",            // ✅ Moved here
    "amount": "15.99",             // ✅ Moved here
    "currency": "usd",
    "granularity": "month",        // ✅ Moved here
    "quantity": 1,                 // ✅ Moved here
    "manual_account_id": 123,
    "plaid_account_id": null
  },
  "overrides": {
    "payee": "Netflix Subscription",  // ✅ Moved here
    "notes": "Monthly subscription",
    "category_id": 456                // ✅ Moved here
  },
  "matches": {
    "request_start_date": "2024-06-01",  // ✅ Renamed from date
    "request_end_date": "2024-06-30",    // ✅ New
    "expected_occurrence_dates": [...],   // ✅ Renamed from occurrences
    "found_transactions": [               // ✅ Renamed from transactions_within_range
      { "date": "2024-06-15", "transaction_id": 789 }
    ],
    "missing_transaction_dates": [...]    // ✅ Renamed from missing_dates_within_range
  }
}
```

#### Query Parameters

```javascript
// v1
GET /v1/recurring_items?start_date=2024-06-01

// v2
GET /v2/recurring?start_date=2024-06-01&end_date=2024-06-30  // Both required if using dates
GET /v2/recurring?include_suggested=true  // Include suggested recurring items
```

**Migration**: 
- Update endpoint path
- Restructure code to access properties in their new locations
- Update property names (`billing_date` → `transaction_criteria.anchor_date`)
- Use both `start_date` and `end_date` when specifying a range

### Manual Accounts (formerly Assets)

#### Endpoint Rename

```javascript
// v1
GET /v1/assets
POST /v1/assets
PUT /v1/assets/:id

// v2
GET /v2/manual_accounts
POST /v2/manual_accounts
PUT /v2/manual_accounts/:id
GET /v2/manual_accounts/:id    // ✅ New: Get single account
DELETE /v2/manual_accounts/:id // ✅ New: Delete account
```

#### Property Renames

```javascript
// v1 → v2
"type_name" → "type"
"subtype_name" → "subtype"
"exclude_transactions" → "exclude_from_transactions"
```

**Note**: Accounts with `type_name: "depository"` now return `type: "cash"`.

#### New Properties

- `updated_at`
- `external_id` (API-only, can be set/updated via API)
- `custom_metadata` (API-only, any valid JSON object < 4MB)

**Migration**: 
- Update all endpoint paths from `/assets` to `/manual_accounts`
- Update property names in your code
- Handle the `depository` → `cash` type change
- Take advantage of new endpoints for single account operations

### Plaid Accounts

#### New Endpoints

```javascript
// v1 - Only list endpoint
GET /v1/plaid_accounts

// v2 - Added single account endpoint
GET /v2/plaid_accounts/:id
```

#### New Properties

- `allow_transaction_modification`: Boolean indicating if transactions can be modified (enabled by default)

#### Fetch Endpoint Changes

```javascript
// v1
POST /v1/plaid_accounts/fetch
// Returns: true

// v2
POST /v2/plaid_accounts/fetch
// Returns: { "plaid_accounts": [...] }  // List of accounts that were fetched
```

**Migration**: Update fetch response handling to expect an object with `plaid_accounts` array instead of `true`.

### Common Migration Patterns

#### Pattern 1: Getting Full Object Details

**v1**: Everything was hydrated
```javascript
const transaction = await getTransaction(123);
console.log(transaction.category_name);  // Direct access
```

**v2**: Fetch related objects separately
```javascript
const transaction = await getTransaction(123);
const category = await getCategory(transaction.category_id);
console.log(category.name);  // Access from category object

// Better: Cache categories
const categoryCache = new Map();
async function getCategoryName(id) {
  if (!categoryCache.has(id)) {
    const cat = await getCategory(id);
    categoryCache.set(id, cat);
  }
  return categoryCache.get(id).name;
}
```

#### Pattern 2: Handling Response Codes

**v1**: All success responses were `200 OK`
```javascript
if (response.status === 200) {
  const data = response.data;  // Might be true, an ID, or an object
}
```

**v2**: Proper HTTP status codes
```javascript
if (response.status === 201) {
  // POST - created object in response.body
  const created = response.body;
} else if (response.status === 200) {
  // PUT - updated object in response.body
  const updated = response.body;
} else if (response.status === 204) {
  // DELETE - no response body
  // Success!
}
```

#### Pattern 3: Error Handling

**v1**: Inconsistent error formats
```javascript
try {
  // Some errors returned as 200 with { error: "message" }
  // Some errors returned as 404
} catch (e) {
  // Handle inconsistently
}
```

**v2**: Consistent error format
```javascript
try {
  const response = await api.post('/transactions', data);
} catch (error) {
  if (error.response.status === 400) {
    const { message, errors } = error.response.body;
    errors.forEach(err => {
      console.error(err.errMsg);
    });
  }
}
```

#### Pattern 4: Creating Tags with Transactions

**v1**: Create tags on the fly
```javascript
POST /v1/transactions
{
  "transactions": [{
    "tags": ["New Tag"]  // Creates tag automatically
  }]
}
```

**v2**: Create tags first
```javascript
// Step 1: Create tag
const tag = await POST /v2/tags { name: "New Tag" };

// Step 2: Use tag ID
POST /v2/transactions
{
  "transactions": [{
    "tag_ids": [tag.id]
  }]
}
```

### Testing Your Migration

1. **Start with Read Operations**: Update your code to read from v2 endpoints first
2. **Handle Dehydration**: Ensure your code can work with IDs instead of hydrated objects
3. **Update Write Operations**: Migrate create/update/delete operations
4. **Test Edge Cases**: 
   - Split transactions
   - Grouped transactions
   - Transactions with recurring items
   - Category groups with children
5. **Verify Error Handling**: Test various error scenarios to ensure proper handling

### Need Help?

If you run into issues during migration:

- Review the [v2 API documentation](../v2/)
- Check the [detailed changelog](../v2/lunch-money-api-changelog.md)
- Reach out to our support team at support@lunchmoney.app

We're here to help make your migration as smooth as possible!

