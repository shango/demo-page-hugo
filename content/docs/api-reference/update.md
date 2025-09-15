---
title: "Update a Workstation"
prev: /docs/stop
next: /docs/delete
weight: 5
sidebar:
  open: true
---
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

{{< tabs items="Example,Schema,Try it" >}}
  {{< tab >}}
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
  {{< /tab >}}
      {{< tab >}}
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
  {{< /tab >}}
  {{< tab >}}

  **Updating workstation name**
    ```bash
    curl -X PATCH "https://api.cloudworkstation.example.com/v1/workstations/ws-12345" \
  -H "Authorization: Bearer <YOUR_TOKEN>" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Renamed-Workstation"
  }'
    ```
  {{< /tab >}}
{{< /tabs >}}

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
