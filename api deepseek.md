# API REST para una Aplicación de Lista de Tareas (ToDo List)

## Descripción General

Esta API permitirá a los usuarios gestionar sus tareas pendientes, incluyendo creación, lectura, actualización y eliminación de tareas, así como otras funcionalidades típicas de una aplicación ToDo.

## Endpoints Base

```
https://api.todoapp.com/v1
```

## Autenticación

Todos los endpoints (excepto `/auth/login` y `/auth/register`) requieren autenticación mediante JWT (JSON Web Token).

## Endpoints

### Autenticación

- **POST /auth/register**
  - Registra un nuevo usuario
  - Body:
    ```json
    {
      "username": "string",
      "email": "string",
      "password": "string"
    }
    ```
  - Respuesta exitosa (201):
    ```json
    {
      "message": "User registered successfully",
      "userId": "string"
    }
    ```

- **POST /auth/login**
  - Inicia sesión y obtiene token JWT
  - Body:
    ```json
    {
      "email": "string",
      "password": "string"
    }
    ```
  - Respuesta exitosa (200):
    ```json
    {
      "token": "string",
      "expiresIn": 3600
    }
    ```

### Tareas

- **GET /tasks**
  - Obtiene todas las tareas del usuario
  - Parámetros opcionales:
    - `status` (pending/completed)
    - `dueDate` (YYYY-MM-DD)
    - `priority` (low/medium/high)
  - Respuesta exitosa (200):
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
  - Crea una nueva tarea
  - Body:
    ```json
    {
      "title": "string",
      "description": "string",
      "dueDate": "YYYY-MM-DD",
      "priority": "low/medium/high"
    }
    ```
  - Respuesta exitosa (201):
    ```json
    {
      "message": "Task created successfully",
      "taskId": "string"
    }
    ```

- **GET /tasks/{id}**
  - Obtiene una tarea específica
  - Respuesta exitosa (200):
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
  - Actualiza una tarea existente
  - Body (campos opcionales):
    ```json
    {
      "title": "string",
      "description": "string",
      "dueDate": "YYYY-MM-DD",
      "priority": "low/medium/high",
      "status": "pending/completed"
    }
    ```
  - Respuesta exitosa (200):
    ```json
    {
      "message": "Task updated successfully"
    }
    ```

- **DELETE /tasks/{id}**
  - Elimina una tarea
  - Respuesta exitosa (200):
    ```json
    {
      "message": "Task deleted successfully"
    }
    ```

- **PATCH /tasks/{id}/complete**
  - Marca una tarea como completada
  - Respuesta exitosa (200):
    ```json
    {
      "message": "Task marked as completed"
    }
    ```

- **PATCH /tasks/{id}/pending**
  - Marca una tarea como pendiente
  - Respuesta exitosa (200):
    ```json
    {
      "message": "Task marked as pending"
    }
    ```

### Categorías (Opcional)

- **GET /categories**
  - Obtiene todas las categorías del usuario
  - Respuesta exitosa (200):
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
  - Crea una nueva categoría
  - Body:
    ```json
    {
      "name": "string",
      "color": "hexColor"
    }
    ```

- **POST /tasks/{id}/categories**
  - Asigna una categoría a una tarea
  - Body:
    ```json
    {
      "categoryId": "string"
    }
    ```

## Códigos de Estado HTTP

- 200 OK
- 201 Created
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 500 Internal Server Error

## Ejemplo de Uso

1. Registrar usuario:
```bash
curl -X POST https://api.todoapp.com/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username":"johndoe","email":"john@example.com","password":"secure123"}'
```

2. Iniciar sesión:
```bash
curl -X POST https://api.todoapp.com/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"secure123"}'
```

3. Crear tarea (usando token JWT):
```bash
curl -X POST https://api.todoapp.com/v1/tasks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer <JWT_TOKEN>" \
  -d '{"title":"Comprar leche","description":"Leche desnatada","dueDate":"2023-12-31","priority":"medium"}'
```

4. Obtener tareas pendientes:
```bash
curl -X GET "https://api.todoapp.com/v1/tasks?status=pending" \
  -H "Authorization: Bearer <JWT_TOKEN>"
```

## Consideraciones Adicionales

1. **Paginación**: Para listados largos, implementar paginación con parámetros `limit` y `offset`.
2. **Búsqueda**: Añadir endpoint `/tasks/search` con parámetro `q` para búsqueda por texto.
3. **Validación**: Validar todos los inputs en el servidor.
4. **Ordenación**: Permitir ordenar resultados por campos como `dueDate` o `priority`.
5. **Rate Limiting**: Implementar límites de solicitudes para prevenir abuso.

Esta API proporciona una base sólida para una aplicación de lista de tareas con funcionalidades esenciales y capacidad de expansión.