# Diseño de API REST para una app web de ToDo List

## Descripción general

Esta API REST permite gestionar tareas (ToDo) para una aplicación web. Está pensada para ser usada desde un frontend desacoplado y puede ser implementada en distintos lenguajes y frameworks backend, siguiendo principios de arquitectura limpia y diseño basado en dominios.

---

## Funcionalidades principales

* Crear nuevas tareas
* Listar todas las tareas
* Editar tareas
* Eliminar tareas
* Marcar tareas como completadas o incompletas
* Agrupar tareas (por usuario, categoría, fecha, etc.)

---

## Tecnologías sugeridas

| Componente    | Alternativas sugeridas                        |
| ------------- | --------------------------------------------- |
| Backend       | Node.js + Express, Go + Gin, Python + FastAPI |
| Base de datos | SQLite                                        |
| Autenticación | JWT (si es multiusuario)                      |

---

## Modelo de datos (`Task`)

```json
{
  "id": "uuid",
  "title": "string",
  "description": "string",
  "completed": false,
  "dueDate": "2025-08-10T18:00:00Z",
  "createdAt": "2025-08-05T20:00:00Z",
  "updatedAt": "2025-08-05T20:00:00Z"
}
```

Si la app es multiusuario:

```json
"userId": "uuid"
```

---

## Endpoints REST

### GET `/tasks`

**Descripción:** Obtener todas las tareas.
**Respuesta exitosa (200):**

```json
[
  {
    "id": "uuid",
    "title": "string",
    "description": "string",
    "completed": false,
    "dueDate": "ISODate",
    "createdAt": "ISODate",
    "updatedAt": "ISODate"
  }
]
```

---

### GET `/tasks/{id}`

**Descripción:** Obtener una tarea por ID.
**Respuesta exitosa (200):** Objeto `Task`
**Errores posibles:** `404 Not Found`

---

### POST `/tasks`

**Descripción:** Crear una nueva tarea.
**Body esperado:**

```json
{
  "title": "string",
  "description": "string",
  "dueDate": "ISODate"
}
```

**Respuesta exitosa (201):** Objeto `Task` creado

---

### PUT `/tasks/{id}`

**Descripción:** Reemplazar completamente una tarea.
**Body esperado:** Objeto `Task` completo
**Respuesta:** `200 OK` con el objeto actualizado

---

### PATCH `/tasks/{id}`

**Descripción:** Actualizar parcialmente una tarea (por ejemplo, cambiar estado de completado).
**Body parcial:**

```json
{
  "completed": true
}
```

**Respuesta:** `200 OK` con la tarea modificada

---

### DELETE `/tasks/{id}`

**Descripción:** Eliminar una tarea.
**Respuesta exitosa:** `204 No Content`
**Errores posibles:** `404 Not Found`

---

## Autenticación (opcional)

Si la aplicación requiere autenticación multiusuario, se sugiere implementar un flujo JWT:

### POST `/auth/register`

**Body esperado:**

```json
{
  "email": "string",
  "password": "string"
}
```

### POST `/auth/login`

**Body esperado:**

```json
{
  "email": "string",
  "password": "string"
}
```

**Respuesta:**

```json
{
  "token": "jwt-token"
}
```

Luego se requiere el uso del token en cada request protegida:

```http
Authorization: Bearer <jwt-token>
```

---

## Ejemplos de uso

### Crear tarea (`POST /tasks`)

```json
{
  "title": "Estudiar Probabilidad",
  "description": "Repasar ejercicios de la guía",
  "dueDate": "2025-08-06T22:00:00Z"
}
```

### Marcar como completada (`PATCH /tasks/{id}`)

```json
{
  "completed": true
}
```

---

## Estructura de proyecto sugerida (Clean Architecture)

```
/app/
│
├── /domain/
│   ├── entities/
│   │   └── task.go / task.ts
│   ├── repositories/
│   │   └── task_repository.go / .ts
│   └── services/
│       └── task_service.go / .ts
│
├── /infrastructure/
│   ├── /persistence/
│   │   └── sqlite_task_repository.go / .ts
│   └── /auth/
│       └── jwt_auth_service.go / .ts
│
├── /interfaces/
│   ├── /http/
│   │   ├── controllers/
│   │   │   └── task_controller.go / .ts
│   │   └── routes/
│   │       └── task_routes.go / .ts
│   └── /middleware/
│       └── auth_middleware.go / .ts
│
├── /config/
│   └── database.go / .ts
│
└── main.go / index.ts
```

> Esta estructura permite portar fácilmente el dominio y la lógica de negocio a diferentes frameworks o lenguajes sin reescribir la app desde cero.
