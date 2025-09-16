---
title: "Retrieve Usage Data"
prev: /docs/stop
next: /docs/delete
toc: false
weight: 6
sidebar:
  open: true
---
### Resource Description
Retrieve CPU, GPU, memory, and billing usage for a workstation.

### Endpoints

```properties
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

{{< tabs items="Example,Schema,Try it" >}}
  {{< tab >}}
    ```json
    {
      "cpuHours": 12.5,
      "gpuHours": 40,
      "memoryGBHours": 256,
      "costUSD": 123.45,
      "lastUpdated": "2025-09-11T12:00:00Z"
    }
    ```
  {{< /tab >}}
  {{< tab >}}
    ```json
    {
      "cpuHours": "float",
      "gpuHours": "integer",
      "memoryGBHours": "integer",
      "costUSD": "float",
      "lastUpdated": "string (date-time)"
    }
    ```
  {{< /tab >}}
  {{< tab >}}
    ```bash
      curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/usage" \
    -H "Authorization: Bearer <YOUR_TOKEN>"
    ```
  {{< /tab >}}
{{< /tabs >}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid workstation ID                           |
| 401    | Unauthorized          | Request not authenticated                        |
| 404    | Not Found             | Workstation ID does not exist                    |
| 500    | Internal Server Error | Failed to retrieve workstation usage data due to system error |
