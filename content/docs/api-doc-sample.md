---
title: ""
type: docs
date: 2025-09-13T19:00:34-06:00
---

# API Reference Documentation Sample

## OpenAPI Specification

For the purposes of this sample, I've created a **minimal OpenAPI 3.1 spec** rendered with **Swagger UI**. The following API Reference Documentation is based upon this specification.

- `openapi.yaml` — the OpenAPI spec (cloud workstation API).
- `index.html` — Swagger UI page that loads and displays the 

{{<cards>}}
  {{<card link="/api-spec-sample" title="Swagger - OpenAPI spec" icon="book-open">}}
{{</cards>}}

## Versioning

All endpoints in this API are versioned under `/v1/`.  
Future versions (`/v2/`, `/v3/`) may introduce breaking changes.  
Clients should specify the version in the path to ensure stability.

Example:
```curl
GET https://api.example.com/v1/workstations
```
## Endpoints
- [Create a Workstation](#post-create-a-new-workstation)
- [Launch a Workstation](#post-launch-a-workstation)
- [Stop a Workstation](#post-stop-a-workstation)
- [Retrieve Usage Data](#get-retrieve-usage-data)
- [Update Workstation](#patch-update-workstation-configuration)

## Authentication
All requests must include a valid API key in the `Authorization` header:

```https
Authorization: Bearer <API_TOKEN>
```

## POST Create a New Workstation
### Resource Description
Provision a new cloud workstation for the authenticated user.
A workstation consists of a CPU, GPU, memory, and storage resources, and can later be launched, stopped, or reconfigured.

Use this endpoint when you want to *add a new workstation* to a project or user account.

## Response Format

This API returns **direct JSON objects** instead of nested envelopes for simplicity and ease of client parsing.

### Endpoint
```https
POST /workstations
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
This operation does not take query or path parameters.
The workstation details are provided in the request body.

### Request Body

{{<tabs items="Example,Schema,Try it">}}
  {{<tab>}}
  ```json
{
  "name": "My-New-Workstation",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
  ```json
{
  "name": "string",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string (GPU Type)",
    "storageGB": "integer",
    "region": "string (Region)"
  }
}
```
  {{</tab>}}
  {{<tab>}}
```curl
curl -X POST "https://api.cloudworkstation.example.com/v1/workstations" \
  -H "Authorization: Bearer <API_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My-New-Workstation",
    "configuration": {
      "cpuCores": 8,
      "memoryGB": 32,
      "gpu": "NVIDIA RTX A5000",
      "storageGB": 512,
      "region": "us-west-2"
    }
  }'
```

  {{</tab>}}
{{</tabs>}}

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 201    | Success               | Returns the newly created workstation object.    |

{{<tabs items="Example,Schema">}}
  {{<tab>}}
  ```json
{
  "id": "ws-12345",
  "name": "My-New-Workstation",
  "status": "provisioning",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
 ```json
{
  "id": "string",
  "name": "string",
  "status": "string (enum: stopped, running, provisioning, error)",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string",
    "storageGB": "integer",
    "region": "string"
  }
}
```
  {{</tab>}}
{{</tabs>}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid input (missing required field or invalid configuration values) |
| 401    | Unauthorized          | Request not authenticated                          |
| 500    | Internal Server Error | Workstation creation failed due to server-side issue |


## POST Launch a Workstation
### Resource Description
Starts a previously provisioned workstation.
The workstation must be in a stopped state before it can be launched. Once launched, it becomes accessible for use until explicitly stopped.

Use this endpoint to power on a workstation for interactive or automated workloads.
### Endpoint
```https
POST /workstations/{id}/launch
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to launch. |

### Request Body
This endpoint does not require a request body.

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation object in its updated state |

{{<tabs items="Example,Schema,Try it">}}
  {{<tab>}}
 ```json
{
  "id": "ws-12345",
  "name": "Artist-Workstation-01",
  "status": "running",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
```json
{
  "id": "string",
  "name": "string",
  "status": "string (enum: stopped, running, provisioning, error)",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string",
    "storageGB": "integer",
    "region": "string"
  }
}
```
  {{</tab>}}
  {{<tab>}}
  ```curl
  curl -X POST "https://api.cloudworkstation.example.com/v1/workstations" \
  -H "Authorization: Bearer <API_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My-New-Workstation",
    "configuration": {
      "cpuCores": 8,
      "memoryGB": 32,
      "gpu": "NVIDIA RTX A5000",
      "storageGB": 512,
      "region": "us-west-2"
    }
  }'
  ```
  {{</tab>}}

{{</tabs>}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID or workstation not in a stopped state  |
| 401    | Unauthorized          | Request not authenticated |
| 404    | Not Found             | Workstation ID not found  |
| 500    | Internal Server Error | Failed to launch the workstation due to system error |


## POST Stop a Workstation
### Resource Description
Power Off a Running Workstation. The workstation must be in a running state before it can be stopped.

Use this endpoint to stop or time-out a workstation.

### Endpoint
```https
POST /workstations/{id}/stop
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to stop. |

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation object in its stopped state |

{{<tabs items="Example,Schema">}}
  {{<tab>}}
```json
{
  "id": "ws-12345",
  "name": "Artist-Workstation-01",
  "status": "stopped",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
```json
{
  "id": "string",
  "name": "string",
  "status": "string (enum: stopped, running, provisioning, error)",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string",
    "storageGB": "integer",
    "region": "string"
  }
}
```
  {{</tab>}}
{{</tabs>}}


### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID or workstation not in a running state |
| 401    | Unauthorized          | Request not authenticated |
| 404    | Not Found             | Workstation ID does not exist |
| 500    | Internal Server Error | Failed to stop the workstation due to system error |


## GET Retrieve Usage Data
### Resource Description
Retrieve CPU, GPU, memory, and billing usage for a workstation.

### Endpoints

```https
GET /workstations/{id}/usage
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to query for usage data. |

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation usage object             |

{{<tabs items="Example,Schema">}}
  {{<tab>}}
```json
{
  "cpuHours": 12.5,
  "gpuHours": 40,
  "memoryGBHours": 256,
  "costUSD": 123.45,
  "lastUpdated": "2025-09-11T12:00:00Z"
}
```
  {{</tab>}}
  {{<tab>}}
```json
{
  "cpuHours": "float",
  "gpuHours": "integer",
  "memoryGBHours": "integer",
  "costUSD": "float",
  "lastUpdated": "string (date-time)"
}
```
  {{</tab>}}
{{</tabs>}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID                           |
| 401    | Unauthorized          | Request not authenticated                        |
| 404    | Not Found             | Workstation ID does not exist                    |
| 500    | Internal Server Error | Failed to retrieve workstation usage data due to system error |


## PATCH Update workstation configuration
### Request Body
Use this endpoint to update the workstation configuration.
The workstation must be in its shutdown state for the configuration to be updated.

This endpoint returns the updated workstation configuration object.

```https
GET /workstations/{id}
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```

### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to update |


{{<tabs items="Example,Schema">}}
  {{<tab>}}
```json
{
  "name": "Renamed-Workstation",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
```json
{
  "name": "string",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string",
    "storageGB": "integer",
    "region": "string (Region)"
  }
}
```
  {{</tab>}}
  {{<tab>}}
  ```json
  curl -X POST "https://api.cloudworkstation.example.com/v1/workstations" \
  -H "Authorization: Bearer <API_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My-New-Workstation",
    "configuration": {
      "cpuCores": 8,
      "memoryGB": 32,
      "gpu": "NVIDIA RTX A5000",
      "storageGB": 512,
      "region": "us-west-2"
    }
  }'
  ```
  {{</tab>}}
{{</tabs>}}

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation configuration has been updated |


{{<tabs items="Example,Schema">}}
  {{< tab >}}
```json
{
  "id": "ws-12345",
  "name": "Artist-Workstation-01",
  "status": "running",
  "configuration": {
    "cpuCores": 8,
    "memoryGB": 32,
    "gpu": "NVIDIA RTX A5000",
    "storageGB": 512,
    "region": "us-west-2"
  }
}
```
  {{</tab>}}
  {{<tab>}}
```json
{
  "id": "string",
  "name": "string",
  "status": "string (Workstation state)",
  "configuration": {
    "cpuCores": "integer",
    "memoryGB": "integer",
    "gpu": "string (GPU Type)",
    "storageGB": "integer",
    "region": "string (Region)"
  }
}
```
  {{</tab>}}
{{</tabs>}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID                           |
| 401    | Unauthorized          | Request not authenticated                        |
| 404    | Not Found             | Workstation ID does not exist                    |
| 500    | Internal Server Error | Failed to update workstation configuration due to system error or workstation was not shut down |
