---
title: "API Reference"
description: "Endpoints for the Cloud Workstation Service API."
toc: false
weight: 1
sidebar:
  open: true
---
## OpenAPI Specification

For the purposes of this sample, I've created a **minimal OpenAPI 3.1 spec** rendered with **Swagger UI**. The following API Reference Documentation is based upon this specification.

- `openapi.yaml` — the OpenAPI spec (cloud workstation API).
- `index.html` — Swagger UI page that loads and displays the 

{{<cards>}}
  {{<card link="/api-spec-sample" title="Swagger - OpenAPI spec" icon="document-text">}}
{{</cards>}}

## Versioning

All endpoints in this API are versioned under `/v1/`.  
Future versions (`/v2/`, `/v3/`) may introduce breaking changes.  
Clients should specify the version in the path to ensure stability.

Example:
```properties
GET https://api.example.com/v1/workstations
```
## Endpoints
- [Create a Workstation](#post-create-a-new-workstation)
- [Launch a Workstation](#post-launch-a-workstation)
- [Stop a Workstation](#post-stop-a-workstation)
- [Retrieve Usage Data](#get-retrieve-usage-data)
- [Update Workstation](#patch-update-workstation-configuration)
- [Delete a Workstation](#delete-remove-a-workstation)

## Authentication
All requests must include a valid API key in the `Authorization` header:

```properties
Authorization: Bearer <API_TOKEN>
```
## Response Format

This API returns **direct JSON objects** instead of nested envelopes for simplicity and ease of client parsing.

