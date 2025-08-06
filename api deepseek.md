# REST API for a ToDo List Application

## Overview

This API will allow users to manage their pending tasks, including creating, reading, updating, and deleting tasks, as well as other typical functionalities of a ToDo application.

## Base Endpoints

```
https://api.todoapp.com/v1
```

## Authentication

All endpoints (except `/auth/login` and `/auth/register`) require authentication using JWT (JSON Web Token).

## Endpoints

### Authentication

- **POST /auth/register**
  - Registers a new user
  - Body:
    ```json
    {
      "username": "string",
      "email": "string",
      "password": "string"
    }
    ```
  - Success response (201):
    ```json
    {
      "message": "User registered successfully",
      "userId": "string"
    }
    ```

- **POST /auth/login**
  - Logs in and obtains JWT token
  - Body:
    ```json
    {
      "email": "string",
      "password": "string"
    }
    ```
  - Success response (200):
    ```json
    {
      "token": "string",
      "expiresIn": 3600
    }
    ```

### Tasks

- **GET /tasks**
  - Gets all user tasks
  - Optional parameters:
    - `status` (pending/completed)
    - `dueDate` (YYYY-MM-DD)
    - `priority` (low/medium/high)
  - Success response (200):
    ```json
    {
      "tasks": [
        {
          "id": "string",
          "title": "string",
          "description": "string",
          "dueDate": "YYYY-MM-DD",
          "priority": "low/medium/high",
          "status": "pending/completed",
          "createdAt": "timestamp",
          "updatedAt": "timestamp"
        }
      ]
    }
    ```

- **POST /tasks**
  - Creates a new task
  - Body:
    ```json
    {
      "title": "string",
      "description": "string",
      "dueDate": "YYYY-MM-DD",
      "priority": "low/medium/high"
    }
    ```
  - Success response (201):
    ```json
    {
      "message": "Task created successfully",
      "taskId": "string"
    }
    ```

- **GET /tasks/{id}**
  - Gets a specific task
  - Success response (200):
    ```json
    {
      "id": "string",
      "title": "string",
      "description": "string",
      "dueDate": "YYYY-MM-DD",
      "priority": "low/medium/high",
      "status": "pending/completed",
      "createdAt": "timestamp",
      "updatedAt": "timestamp"
    }
    ```

- **PUT /tasks/{id}**
  - Updates an existing task
  - Body (optional fields):
    ```json
    {
      "title": "string",
      "description": "string",
      "dueDate": "YYYY-MM-DD",
      "priority": "low/medium/high",
      "status": "pending/completed"
    }
    ```
  - Success response (200):
    ```json
    {
      "message": "Task updated successfully"
    }
    ```

- **DELETE /tasks/{id}**
  - Deletes a task
  - Success response (200):
    ```json
    {
      "message": "Task deleted successfully"
    }
    ```

- **PATCH /tasks/{id}/complete**
  - Marks a task as completed
  - Success response (200):
    ```json
    {
      "message": "Task marked as completed"
    }
    ```

- **PATCH /tasks/{id}/pending**
  - Marks a task as pending
  - Success response (200):
    ```json
    {
      "message": "Task marked as pending"
    }
    ```

### Categories (Optional)

- **GET /categories**
  - Gets all user categories
  - Success response (200):
    ```json
    {
      "categories": [
        {
          "id": "string",
          "name": "string",
          "color": "hexColor"
        }
      ]
    }
    ```

- **POST /categories**
  - Creates a new category
  - Body:
    ```json
    {
      "name": "string",
      "color": "hexColor"
    }
    ```

- **POST /tasks/{id}/categories**
  - Assigns a category to a task
  - Body:
    ```json
    {
      "categoryId": "string"
    }
    ```

## HTTP Status Codes

- 200 OK
- 201 Created
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 500 Internal Server Error

## Usage Example

1. Register user:
```bash
curl -X POST https://api.todoapp.com/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"johndoe","email":"john@example.com","password":"secure123"}'
```

2. Login:
```bash
curl -X POST https://api.todoapp.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"secure123"}'
```

3. Create task (using JWT token):
```bash
curl -X POST https://api.todoapp.com/v1/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -d '{"title":"Buy milk","description":"Skim milk","dueDate":"2023-12-31","priority":"medium"}'
```

4. Get pending tasks:
```bash
curl -X GET "https://api.todoapp.com/v1/tasks?status=pending" \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

## Additional Considerations

1. **Pagination**: For long lists, implement pagination with `limit` and `offset` parameters.
2. **Search**: Add `/tasks/search` endpoint with `q` parameter for text search.
3. **Validation**: Validate all inputs on the server.
4. **Sorting**: Allow sorting results by fields like `dueDate` or `priority`.
5. **Rate Limiting**: Implement request limits to prevent abuse.

This API provides a solid foundation for a ToDo list application with essential functionalities and expansion capabilities.
