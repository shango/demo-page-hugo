---
title: "Delete a Workstation"
weight: 7
sidebar:
  open: true
---
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