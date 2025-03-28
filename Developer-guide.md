# Fireblocks-test
Fireblocks test spec
# Technical Writer Tasks API Developer Guide	

## Introduction
Follow this guide to use the Technical Writer Tasks API

### Overview
The Technical Writer Tasks API allows users to keep track of tasks as a technical writer.  With this API, users can retrieve existing tasks and create new ones, categorizing them by status and component.

## Authentication

An API key is required for all requests.  The API key must be entered in the headers;

**Example header:**
```sh
 X-API-KEY: your_api_key_here  
 ```

The organization’s API coordinator can provide you the necessary key.  Do not share your API key publicly.  

## Endpoints

### GET/tasks
Retrieves a list of technical writer tasks.  Optional filters for status, component, and date of last update.

-**HTTP:** GET
-**URI:** https://api.techwriter.xyz/tasks

#### Header
-	X-API-KEY : enter your API key here
  
#### Query parameters

-	`Status`: Filter tasks based on status i.e (OPEN, IN_PROGRESS, COMPLETE)
-	`Component`: Filter tasks by documentation component (SKD_DOCS, HELP_DESK, etc)
-	`updatedAfter`:  Filter tasks updated after a specific date (YYYY-MM-DD)

#### Request Example
```bash
curl https://api.techwriter.xyz/tasks \
  -"X-API-KEY: your_api_key_here" \
  -\
  -"status=OPEN" \
  -"component=API_DOCS" 
```
 
#### Status Codes

-`200` : Request Successful, array of tasks shown

-`401` : Failed Authentication, Invalid API Key.

##### Success response example (200)
```json
[
  {
    "task_id": "123",
    "title": "Update API Documentation",
    "description": "Revise the authentication guide.",
    "status": "IN_PROGRESS",
    "component": "API_DOCS",
    "connected_tasks": []
  }
]
```
#####Error example (401)

```json
{
  "error": "Invalid or missing API key."
}
```

### POST/Task

Create new technical writing task.

-**HTTP:** POST
-**URI:** https://api.techwriter.xyz/task

#### Headers
-`X-API-KEY` : enter your API key here
-`Content-Type` : application/json

####Request Body

```json
{
  "title": "Write SDK Documentation",
  "description": "Document the latest SDK release.",
  "status": "OPEN",
  "component": "SDK_DOCS",
  "connected_tasks": [
    {
      "task_id": "101",
      "title": "Review API Docs",
      "description": "Ensure API docs are up to date.",
      "status": "IN_PROGRESS"
    }
  ]
}
```


#### Request Example

```bash
curl https://api.techwriter.xyz/task \
  -POST \
  -"X-API-KEY: your_api_key" \
  -"Content-Type: application/json" \
  -'{
    "title": "Write SDK Documentation",
    "description": "Document the latest SDK release.",
    "status": "OPEN",
    "component": "SDK_DOCS",
    "connected_tasks": [
```

#### Status Codes 
-`201`: Task created successfully

-`400`: Invalid Task Data

-`401`: Failed Authentication, Invalid API Key.

##### Success response example (201)

```json
{
  "task_id": "task_131"
  "title": "Write SDK Documentation"
  "description": "Document the latest SDK release"
  "Status": "OPEN"
  "Component": "SDK_DOCS"
  "Connected_tasks": ["task_id_101"]
}
```

#### Error examples

###### **400**
```json
{
  "error": "Invalid task data"
}
```

###### 401
```json
{
  "error": "Invalid API key"
}
```

#### Code Example (Python)
```python
import requests

API_KEY = 'your_api_key_here'
BASE_URL = 'https://api.techwriter.xyz'

headers = {
    "X-API-KEY": API_KEY,
    "Content-Type": "application/json"
}

data = {
    "title": "Write SDK Documentation",
    "description": "Document the latest SDK release.",
    "status": "OPEN",
    "component": "SDK_DOCS",
    "connected_tasks": []
}

response = requests.post(f"{BASE_URL}/tasks", json=data, headers=headers)

print(response.status_code, response.json())
```

## Best Practices

1.	Keep API Key Secure – Never allow you’re API key to be exposed in a public repository.
2.	Handle errors with grace – Check response status codes and implement retry logic for transient errors.
3.	Implement Filtering and pagination– Use query parameters to retrieve only relevant task, thus improving performance.
4.	Use JSON Web Tokens – JWT can help improve security of your API.
5.	Utilize Role-Based Access Control (RBAC) – RBAC lets you control what different users can access or edit.

