# Lunch Money API Documentation (Scalar Pro)

This repository contains the source for the new Lunch Money API documentation portal, powered by [Scalar Pro](https://guides.scalar.com/scalar/scalar-docs/getting-started).

It's not clear to me how Scalar uses this readme.

## Structure

- `guides/` — All documentation guides, including migrated v1 guides and new v2 content.
- `api/` — OpenAPI specs for v2 (and v1 if available), for import into Scalar Pro.
- `static/` — Images and other static assets.

## Goals
- **v2 API is front and center**: The v2 OpenAPI spec and guides are the default entry point for new users.
- **v1 API documentation is preserved**: All v1 guides are available and clearly marked as legacy, with banners suggesting new projects use v2.
- **Migration support**: Includes a migration guide for users moving from v1 to v2.

## How to Use
- Import the OpenAPI spec from `api/lunch-money-api-v2.yaml` into Scalar Pro for an interactive API reference.
- Use the guides in `guides/` for onboarding, usage, and migration information.
- Static assets can be referenced from the `static/` directory as needed.

## Next Steps
- Link this repo to your Scalar Pro account for GitHub sync.
- Continue to update and improve guides as the API evolves.
