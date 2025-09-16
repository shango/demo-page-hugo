---
title: "Stop Workstation"
prev: /docs/launch
next: /docs/update
toc: false
weight: 4
sidebar:
  open: true
---
### Resource Description
Stop a Running Workstation. The workstation must be in a running state before it can be stopped.

Use this endpoint to stop or time-out a workstation.

### Endpoint
```properties
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

{{< tabs items="Example,Schema,Try it" >}}
  {{< tab >}}
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
  {{< /tab >}}
  {{< tab >}}
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
  {{< /tab >}}
  {{< tab >}}
  ```bash
    curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/stop" \
-H "Authorization: Bearer <YOUR_TOKEN>"
  ```
  {{< /tab >}}
{{< /tabs >}}


### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID or workstation not in a running state |
| 401    | Unauthorized          | Request not authenticated |
| 404    | Not Found             | Workstation ID does not exist |
| 500    | Internal Server Error | Failed to stop the workstation due to system error |
