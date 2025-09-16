---
title: "API Reference"
description: "Endpoints for the Cloud Workstation Service API."
toc: false
weight: 1
sidebar:
  open: true
---
This documentation describes the **Cloud Workstation API**, a sample REST API that enables users to provision and manage cloud-based workstations.

For this writing sample, I designed and structured the OpenAPI 3.1 specification and then generated a [Swagger UI page](/api-spec-sample) with interactive examples and testing capabilities.
This sample demonstrates my experience with both the technical side of API specification design and my ability to transform those specs into clear, developer-focused documentation.

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
- [Create Workstation](create)
- [Launch Workstation](launch)
- [Stop Workstation](stop)
- [Retrieve Usage Data](usage)
- [Update Workstation](update)
- [Delete Workstation](delete)

## Authentication
All requests must include a valid API key in the `Authorization` header:

```properties
Authorization: Bearer <API_TOKEN>
```
## Response Format

This API returns **direct JSON objects** instead of nested envelopes for simplicity and ease of client parsing.

### Example Response:
```json
{
  "id": "ws-12345",
  "name": "My Workstation",
  "status": "running",
  "cpuCores": 8,
  "memoryGB": 32
}
```
## Errors

If a request cannot be completed, the API returns an error response with both a machine-readable code and a human-readable message.

### Example Error Response:
```json
{
  "error": {
    "code": "INVALID_CONFIG",
    "message": "CPU cores must be >= 2"
  }
}
```
