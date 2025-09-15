---
title: "Create a Workstation"
prev: /docs/api-reference
next: /docs/launch
weight: 2
sidebar:
  open: true
---
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

{{< tabs items="Example,Schema,Try it" >}}
  {{< tab >}}
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
  {{< /tab >}}
  {{< tab >}}
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
  {{< /tab >}}
  {{< tab >}}
  ```shell
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
  {{< /tab >}}
{{< /tabs >}}

### Response
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 201    | Success               | Returns the newly created workstation object.    |

{{< tabs items="Example,Schema" >}}
  {{< tab >}}
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
{{< /tabs >}}

### Error Responses
| Status | Meaning               | Description                                      |
|--------|-----------------------|--------------------------------------------------|
| 400    | Bad Request           | Invalid input (missing required field or invalid configuration values) |
| 401    | Unauthorized          | Request not authenticated                          |
| 500    | Internal Server Error | Workstation creation failed due to server-side issue |