# REST API Design for a ToDo List Web App

## Overview

This REST API allows the management of tasks (ToDos) for a web application. It is designed to work with a decoupled frontend and to be implemented in any backend stack, following Clean Architecture and Domain-Driven Design (DDD) principles.

---

## Core Features

* Create new tasks
* List all tasks
* Edit tasks
* Delete tasks
* Mark tasks as completed or not completed
* Group tasks (by user, category, date, etc.)

---

## Suggested Technologies

| Component      | Alternatives                                                    |
| -------------- | --------------------------------------------------------------- |
| Backend        | Any modern framework (e.g., Express, FastAPI, Gin, Spring Boot) |
| Database       | SQLite                                                          |
| Authentication | JWT (optional, for multi-user support)                          |

---

## Domain Model: `Task`

A task is the core aggregate root in the domain.

### Attributes

| Field       | Type     | Description                       |
| ----------- | -------- | --------------------------------- |
| id          | UUID     | Unique identifier                 |
| title       | String   | Short title of the task           |
| description | String   | Detailed description              |
| completed   | Boolean  | Whether the task is completed     |
| dueDate     | DateTime | Optional deadline                 |
| createdAt   | DateTime | Creation timestamp                |
| updatedAt   | DateTime | Last modification timestamp       |
| userId\*    | UUID     | Owner of the task (if multi-user) |

> \*Optional, only used in multi-user systems.

---

## REST Endpoints

### `GET /tasks`

**Description:** Retrieve a list of all tasks.
**Response:**

* `200 OK`: Array of `Task` objects

---

### `GET /tasks/{id}`

**Description:** Retrieve a task by its ID.
**Response:**

* `200 OK`: `Task` object
* `404 Not Found`: If the task does not exist

---

### `POST /tasks`

**Description:** Create a new task.
**Request Body:**

```json
{
  "title": "string",
  "description": "string",
  "dueDate": "ISODate"
}
```

**Response:**

* `201 Created`: Newly created `Task` object

---

### `PUT /tasks/{id}`

**Description:** Replace a task entirely.
**Request Body:** Full `Task` object
**Response:**

* `200 OK`: Updated `Task` object

---

### `PATCH /tasks/{id}`

**Description:** Partially update a task (e.g., toggle completion).
**Request Body:**

```json
{
  "completed": true
}
```

**Response:**

* `200 OK`: Updated `Task` object

---

### `DELETE /tasks/{id}`

**Description:** Delete a task.
**Response:**

* `204 No Content`: Successfully deleted
* `404 Not Found`: If the task does not exist

---

## Authentication (Optional)

### `POST /auth/register`

**Request Body:**

```json
{
  "email": "string",
  "password": "string"
}
```

### `POST /auth/login`

**Request Body:**

```json
{
  "email": "string",
  "password": "string"
}
```

**Response:**

```json
{
  "token": "jwt-token"
}
```

**Authorization header for protected requests:**

```http
Authorization: Bearer <jwt-token>
```

---

## Usage Examples

### Create a task

```json
POST /tasks
{
  "title": "Study Probability",
  "description": "Review exercises from the guide",
  "dueDate": "2025-08-06T22:00:00Z"
}
```

### Mark task as completed

```json
PATCH /tasks/{id}
{
  "completed": true
}
```

---

## Suggested Project Structure (Domain-Oriented, Technology-Agnostic)

```
/app/
│
├── /domain/
│   ├── /entities/         # Core business entities (e.g., Task)
│   ├── /value_objects/    # Reusable typed objects (e.g., Email, DateRange)
│   ├── /repositories/     # Interfaces for persistence abstraction
│   └── /services/         # Domain services (pure business logic)
│
├── /application/
│   ├── /use_cases/        # Application-level services (e.g., CreateTask)
│   └── /dto/              # Input/output data transfer objects
│
├── /infrastructure/
│   ├── /persistence/      # Implementations of repository interfaces (e.g., SQLite)
│   └── /auth/             # JWT handling, password hashing
│
├── /interfaces/
│   ├── /http/
│   │   ├── /controllers/  # Route handlers for HTTP requests
│   │   └── /routes/       # Route declarations
│   └── /middleware/       # Auth, error handling, logging
│
├── /config/               # App configuration, database setup
│
└── /main/                 # Entry point (e.g., server startup)
```

> This structure cleanly separates concerns and allows reuse of domain logic across different interfaces (e.g., CLI, HTTP, gRPC).
