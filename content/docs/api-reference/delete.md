---
title: "Delete Workstation"
prev: /docs/update
toc: false
weight: 7
sidebar:
  open: true
---
### Resource Description
Use this endpoint to permanently remove a workstation from the system. Deletion detaches and removes any associated compute resources. Persistent storage volumes are also deleted unless they were explicitly created as standalone resources.

> **Note:** The workstation must be in a **stopped** state before it can be deleted.  
Requests to update a running workstation will return a `400 Bad Request` error.

### Endpoints
```properties
GET /workstations/{id}/delete
Host: api.cloudworkstation.example.com/v1/
Content-Type: application/json
Accept: application/json
```
### Parameters
| Name | In   | Type   | Required | Description                             |
| ---- | ---- | ------ | -------- | --------------------------------------- |
| `id` | path | string | Yes      | Unique ID of the workstation to query to delete |

### Request Body
This endpoint does not require a request body.

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 204    | No Content               | Returns the workstation delete object           |

{{< tabs items="Example,Schema,Try it" >}}
  {{< tab >}}
```json
{
  "id": "ws-12345",
  "status": "deleted"
}
```
  {{< /tab >}}
  {{< tab >}}
```json
{
  "id": "string",
  "status": "string (deleted, error)"
}
```
  {{< /tab >}}
  {{< tab >}}
  ```bash
    curl -X GET "https://api.cloudworkstation.example.com/v1/workstations/ws-12345/delete" \
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
