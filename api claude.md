# ToDo List REST API Specification

## Base URL
```
https://api.todoapp.com/v1
```

## Data Models

### User
```json
{
  "id": "uuid",
  "username": "string",
  "email": "string",
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

### Todo Item
```json
{
  "id": "uuid",
  "title": "string",
  "description": "string",
  "completed": "boolean",
  "priority": "enum[low, medium, high]",
  "due_date": "datetime",
  "created_at": "datetime",
  "updated_at": "datetime",
  "user_id": "uuid",
  "category_id": "uuid"
}
```

### Category
```json
{
  "id": "uuid",
  "name": "string",
  "color": "string",
  "user_id": "uuid",
  "created_at": "datetime",
  "updated_at": "datetime"
}
```

## Authentication

All endpoints require authentication via JWT tokens passed in the Authorization header:
```
Authorization: Bearer <jwt_token>
```

### Auth Endpoints

#### POST /auth/register
Register a new user account.

**Request Body:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

**Response (201):**
```json
{
  "user": {
    "id": "uuid",
    "username": "string",
    "email": "string",
    "created_at": "datetime"
  },
  "access_token": "string",
  "refresh_token": "string"
}
```

#### POST /auth/login
Authenticate user and get access token.

**Request Body:**
```json
{
  "email": "string",
  "password": "string"
}
```

**Response (200):**
```json
{
  "user": {
    "id": "uuid",
    "username": "string",
    "email": "string"
  },
  "access_token": "string",
  "refresh_token": "string"
}
```

#### POST /auth/refresh
Refresh access token using refresh token.

**Request Body:**
```json
{
  "refresh_token": "string"
}
```

**Response (200):**
```json
{
  "access_token": "string"
}
```

#### POST /auth/logout
Invalidate current session.

**Response (204):** No content

## Todo Endpoints

#### GET /todos
Get all todos for the authenticated user.

**Query Parameters:**
- `completed` (boolean): Filter by completion status
- `priority` (string): Filter by priority (low, medium, high)
- `category_id` (uuid): Filter by category
- `due_date_from` (datetime): Filter todos due after this date
- `due_date_to` (datetime): Filter todos due before this date
- `page` (integer): Page number for pagination (default: 1)
- `limit` (integer): Items per page (default: 20, max: 100)
- `sort` (string): Sort field (created_at, updated_at, due_date, priority)
- `order` (string): Sort order (asc, desc, default: desc)

**Response (200):**
```json
{
  "todos": [
    {
      "id": "uuid",
      "title": "string",
      "description": "string",
      "completed": false,
      "priority": "high",
      "due_date": "2024-12-31T23:59:59Z",
      "created_at": "datetime",
      "updated_at": "datetime",
      "category": {
        "id": "uuid",
        "name": "Work",
        "color": "#ff5722"
      }
    }
  ],
  "pagination": {
    "current_page": 1,
    "per_page": 20,
    "total_items": 45,
    "total_pages": 3,
    "has_next": true,
    "has_prev": false
  }
}
```

#### POST /todos
Create a new todo item.

**Request Body:**
```json
{
  "title": "string",
  "description": "string",
  "priority": "enum[low, medium, high]",
  "due_date": "datetime",
  "category_id": "uuid"
}
```

**Response (201):**
```json
{
  "id": "uuid",
  "title": "string",
  "description": "string",
  "completed": false,
  "priority": "high",
  "due_date": "datetime",
  "created_at": "datetime",
  "updated_at": "datetime",
  "category": {
    "id": "uuid",
    "name": "Work",
    "color": "#ff5722"
  }
}
```

#### GET /todos/{id}
Get a specific todo item.

**Response (200):**
```json
{
  "id": "uuid",
  "title": "string",
  "description": "string",
  "completed": false,
  "priority": "high",
  "due_date": "datetime",
  "created_at": "datetime",
  "updated_at": "datetime",
  "category": {
    "id": "uuid",
    "name": "Work",
    "color": "#ff5722"
  }
}
```

#### PUT /todos/{id}
Update a todo item completely.

**Request Body:**
```json
{
  "title": "string",
  "description": "string",
  "completed": "boolean",
  "priority": "enum[low, medium, high]",
  "due_date": "datetime",
  "category_id": "uuid"
}
```

**Response (200):** Updated todo object

#### PATCH /todos/{id}
Partially update a todo item.

**Request Body:** (any subset of updatable fields)
```json
{
  "completed": true
}
```

**Response (200):** Updated todo object

#### DELETE /todos/{id}
Delete a todo item.

**Response (204):** No content

## Category Endpoints

#### GET /categories
Get all categories for the authenticated user.

**Response (200):**
```json
{
  "categories": [
    {
      "id": "uuid",
      "name": "Work",
      "color": "#ff5722",
      "created_at": "datetime",
      "updated_at": "datetime",
      "todo_count": 12
    }
  ]
}
```

#### POST /categories
Create a new category.

**Request Body:**
```json
{
  "name": "string",
  "color": "string"
}
```

**Response (201):** Created category object

#### GET /categories/{id}
Get a specific category.

**Response (200):** Category object with todo_count

#### PUT /categories/{id}
Update a category.

**Request Body:**
```json
{
  "name": "string",
  "color": "string"
}
```

**Response (200):** Updated category object

#### DELETE /categories/{id}
Delete a category. Todos in this category will have their category_id set to null.

**Response (204):** No content

## Bulk Operations

#### POST /todos/bulk
Create multiple todos at once.

**Request Body:**
```json
{
  "todos": [
    {
      "title": "string",
      "description": "string",
      "priority": "medium",
      "category_id": "uuid"
    }
  ]
}
```

**Response (201):**
```json
{
  "created": [
    /* array of created todo objects */
  ],
  "errors": [
    {
      "index": 2,
      "error": "Title is required"
    }
  ]
}
```

#### PATCH /todos/bulk
Update multiple todos at once.

**Request Body:**
```json
{
  "updates": [
    {
      "id": "uuid",
      "completed": true
    },
    {
      "id": "uuid",
      "priority": "high"
    }
  ]
}
```

**Response (200):**
```json
{
  "updated": [
    /* array of updated todo objects */
  ],
  "errors": [
    {
      "id": "uuid",
      "error": "Todo not found"
    }
  ]
}
```

#### DELETE /todos/bulk
Delete multiple todos.

**Request Body:**
```json
{
  "ids": ["uuid1", "uuid2", "uuid3"]
}
```

**Response (200):**
```json
{
  "deleted_count": 3,
  "errors": []
}
```

## Statistics Endpoints

#### GET /stats/overview
Get overview statistics for the user.

**Response (200):**
```json
{
  "total_todos": 45,
  "completed_todos": 32,
  "pending_todos": 13,
  "overdue_todos": 5,
  "completion_rate": 0.71,
  "categories_count": 6,
  "todos_created_today": 3,
  "todos_completed_today": 8
}
```

#### GET /stats/productivity
Get productivity statistics over time.

**Query Parameters:**
- `period` (string): Time period (week, month, year)
- `start_date` (date): Start date for custom range
- `end_date` (date): End date for custom range

**Response (200):**
```json
{
  "period": "month",
  "data": [
    {
      "date": "2024-01-01",
      "created": 5,
      "completed": 7
    }
  ]
}
```

## Error Responses

All error responses follow this format:

**4xx/5xx Response:**
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "The request data is invalid",
    "details": {
      "title": ["Title is required"],
      "due_date": ["Due date must be in the future"]
    }
  },
  "timestamp": "2024-01-15T10:30:00Z",
  "path": "/todos"
}
```

## Common HTTP Status Codes

- `200 OK`: Successful GET, PUT, PATCH
- `201 Created`: Successful POST
- `204 No Content`: Successful DELETE
- `400 Bad Request`: Invalid request data
- `401 Unauthorized`: Missing or invalid authentication
- `403 Forbidden`: User doesn't have permission
- `404 Not Found`: Resource doesn't exist
- `409 Conflict`: Resource conflict (e.g., duplicate email)
- `422 Unprocessable Entity`: Validation errors
- `429 Too Many Requests`: Rate limit exceeded
- `500 Internal Server Error`: Server error

## Rate Limiting

- 1000 requests per hour per user for authenticated endpoints
- 100 requests per hour per IP for auth endpoints
- Rate limit headers included in responses:
  - `X-RateLimit-Limit`: Request limit
  - `X-RateLimit-Remaining`: Remaining requests
  - `X-RateLimit-Reset`: Reset timestamp

## Versioning

API versioning is handled through URL paths (`/v1/`). Older versions will be supported for at least 12 months after a new version is released.

## WebSocket Support (Optional)

For real-time updates:
```
wss://api.todoapp.com/v1/ws?token=<jwt_token>
```

Events:
- `todo.created`
- `todo.updated`
- `todo.deleted`
- `category.created`
- `category.updated`
- `category.deleted`