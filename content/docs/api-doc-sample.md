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

```bash
Authorization: Bearer <API_TOKEN>
```
## Response Format

This API returns **direct JSON objects** instead of nested envelopes for simplicity and ease of client parsing.


## POST Create a New Workstation
### Resource Description
Provision a new cloud workstation for the authenticated user.
A workstation consists of a CPU, GPU, memory, and storage resources, and can later be launched, stopped, or reconfigured.

Use this endpoint when you want to *add a new workstation* to a project or user account.

### Endpoint
```bash
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
```bash
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
```bash
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
  ```bash
    curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/launch" \
-H "Authorization: Bearer <YOUR_TOKEN>"
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
Stop a Running Workstation. The workstation must be in a running state before it can be stopped.

Use this endpoint to stop or time-out a workstation.

### Endpoint
```bash
POST /workstations/{id}/stop
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to stop. |

### Request Body
This endpoint does not require a request body.

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation object in its stopped state |

{{<tabs items="Example,Schema,Try it">}}
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
  {{<tab>}}
  ```bash
    curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/stop" \
-H "Authorization: Bearer <YOUR_TOKEN>"
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

```bash
GET /workstations/{id}/usage
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to query for usage data. |

### Request Body
This endpoint does not require a request body.

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the workstation usage object             |

{{<tabs items="Example,Schema,Try it">}}
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
  {{<tab>}}
    ```bash
      curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/usage" \
    -H "Authorization: Bearer <YOUR_TOKEN>"
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
### Resource Description
Update the name or configuration of an existing workstation.  
This endpoint supports **partial updates** — only include the fields you want to change.  
All unspecified fields remain unchanged.  

> **Note:** The workstation must be in a **stopped** state before updates are allowed.  
Requests to update a running workstation will return a `400 Bad Request` error.

```bash
PATCH /workstations/{id}
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```

### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to update |

### Request Body

{{<tabs items="Example,Schema,Try it">}}
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
  **Updating workstation name**
    ```bash
    curl -X PATCH "https://api.cloudworkstation.example.com/v1/workstations/ws-12345" \
  -H "Authorization: Bearer <YOUR_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Renamed-Workstation"
  }'
    ```
  {{</tab>}}
{{</tabs>}}

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 200    | Success               | Returns the updated workstation object           |


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
| 401    | Unauthorized          | Invalid workstation ID, invalid configuration values, or workstation not in a stopped state. |
| 404    | Not Found             | Workstation ID does not exist                    |
| 500    | Internal Server Error | Failed to update workstation due to system error


## PATCH Update workstation configuration
### Resource Description
Delete the workstation

### Endpoints

```bash
GET /workstations/{id}/delete
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to query to delete. |

### Request Body
This endpoint does not require a request body.

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 204    | Success               | Returns the workstation delete object.             |