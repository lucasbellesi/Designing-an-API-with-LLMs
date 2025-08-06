# 📋 Comparando Diseños de API REST para una App To-Do: ChatGPT vs DeepSeek

> ¿Cómo diseñaría una API para una aplicación de tareas **ChatGPT** frente a **DeepSeek**?  
> Este repositorio compara dos enfoques generados por modelos de lenguaje avanzados, analizando estructura, claridad, buenas prácticas y preparación para producción.

---

## 📂 Estructura del Repositorio

```
Designing-an-API-with-LLMs/
├── api-chatgpt.md     # Diseño generado por ChatGPT
├── api-deepseek.md    # Diseño generado por DeepSeek
└── README.md          # Este archivo: comparación y análisis
```

Ambos diseños son APIs REST para una **aplicación web de gestión de tareas (To-Do List)**, pero con enfoques claramente distintos.

---

## 🎯 Objetivo del Proyecto

Este repositorio explora cómo dos LLMs líderes — **ChatGPT** y **DeepSeek** — responden a la misma solicitud: *“Diseña una API REST para una app web de To-Do”*.

El foco está en comparar:
- Calidad técnica y adherencia a REST.
- Claridad de la documentación.
- Escalabilidad y preparación para entornos reales.
- Enfoque arquitectónico y experiencia de desarrollador.

---

## 🔍 Comparativa Técnica Detallada

| Característica               | **ChatGPT** | **DeepSeek** |
|-----------------------------|-------------|--------------|
| **Estilo de diseño**         | Educativo, detallado, con principios de DDD y Clean Architecture | Directo, técnico, orientado a implementación rápida |
| **Enfoque arquitectónico**   | ✅ Domain-Driven Design (DDD) y separación clara de capas | ⚠️ Funcional, pero sin estructura de proyecto detallada |
| **Modelo de `Task`**         | Atributos claros: `id`, `title`, `description`, `completed`, `dueDate`, `timestamps`, `userId` (opcional) | Incluye `priority` y `status` (más detallado que booleano) |
| **Autenticación**            | Propone JWT con `/auth/register` y `/auth/login` | Implementa JWT con respuestas detalladas (incluye `expiresIn`) |
| **Endpoints clave**          | `POST`, `GET`, `PUT`, `PATCH`, `DELETE` en `/tasks/{id}` | Igual, pero con endpoints específicos: `PATCH /tasks/{id}/complete` y `PATCH /tasks/{id}/pending` |
| **Filtros y búsquedas**      | Menciona agrupación (por usuario, categoría, fecha), pero sin especificar query params | ✅ `status`, `dueDate`, `priority` como parámetros opcionales en `GET /tasks` |
| **Categorías**               | Mencionadas como posibilidad, sin endpoints | ✅ Endpoints completos: `GET /categories`, `POST /categories`, y asignación a tareas |
| **Respuestas de éxito**      | Devuelve el objeto `Task` completo (estándar REST) | Usa mensajes como `{ "message": "Task created successfully" }` (menos RESTful) |
| **Códigos de estado HTTP**   | Correctos y bien usados (`201 Created`, `204 No Content`) | Correctos, pero usa `200` en `DELETE` (debería ser `204`) |
| **Documentación de uso**     | Ejemplos de request claros | ✅ Incluye `curl` completo con headers y JWT |
| **Consideraciones avanzadas**| Menciona DDD, Clean Architecture, DTOs, repositorios | ✅ Paginación, búsqueda, ordenamiento, rate limiting |
| **Proyecto listo para código**| ✅ Estructura de carpetas detallada (ideal para equipos) | ⚠️ Listo para implementar, pero sin guía de arquitectura |

---

## 🛠️ Funcionalidades Clave

### ✅ Comunes a ambos
- Crear, leer, actualizar y eliminar tareas.
- Marcar tareas como completadas (con `PATCH`).
- Autenticación con JWT.
- Validación de entrada y manejo de errores.

### ✅ Ventajas de ChatGPT
- **Arquitectura limpia**: separa dominio, aplicación, infraestructura e interfaces.
- **Enfoque profesional**: ideal para equipos que priorizan mantenibilidad.
- **Mejor adherencia a REST**: devuelve recursos, no solo mensajes.
- **Estructura de proyecto detallada** (DDD): fácil de escalar.

### ✅ Ventajas de DeepSeek
- **Funcionalidades extra**: `priority`, `status`, categorías y colores.
- **Endpoints especializados**: `complete` y `pending` separados (más intuitivos).
- **Uso de `curl`**: excelente para documentación práctica.
- **Consideraciones de producción**: paginación, búsqueda, rate limiting.

---

## 🧠 Análisis de Estilo y Calidad

| Aspecto | ChatGPT | DeepSeek |
|-------|-------|--------|
| **Claridad** | Alta, con explicaciones y contexto | Alta, pero más densa |
| **Profundidad técnica** | Profunda (DDD, DTOs, repositorios) | Práctica (enfocada en funcionalidad) |
| **Escalabilidad** | Excelente (listo para múltiples usuarios y servicios) | Buena, pero menos modular |
| **Experiencia de desarrollador** | Ideal para onboarding | Ideal para prototipado rápido |

> 💡 **Conclusión**:  
> - **ChatGPT** es mejor si buscas una base sólida, mantenible y profesional.  
> - **DeepSeek** es mejor si necesitas funcionalidades rápidas y una API lista para usar en un MVP.

---

## 📚 Cómo Usar este Repositorio

1. **Compara ambos diseños**:
   - Abre `api-chatgpt.md` y `api-deepseek.md`.
   - Analiza cómo cada modelo define recursos, relaciones y flujos.

2. **Elige tu enfoque**:
   - ¿Prefieres arquitectura limpia o funcionalidad rápida?
   - ¿Necesitas categorías y prioridades desde el inicio?

3. **Implementa una API real**:
   - Usa uno de los diseños como base.
   - Ejemplo con FastAPI o Express + SQLite.

4. **Extiéndelo**:
   - Añade OpenAPI (Swagger) basado en uno de los diseños.
   - Implementa el frontend (React, Vue).
   - Compara tiempos de desarrollo.

---

## 💡 Ideas para Profundizar

- ✅ Generar un archivo `openapi.yaml` para cada diseño.
- ✅ Implementar ambas APIs y comparar esfuerzo de desarrollo.
- ✅ Hacer un benchmark de claridad con otros desarrolladores.
- ✅ Añadir pruebas unitarias y de integración.
- ✅ Comparar con otros LLMs: Claude, Gemini, Mistral.

---

## 🤝 Contribuciones

¿Quieres mejorar este análisis? ¿Agregar un tercer modelo o una implementación funcional?  
¡Tu aporte es bienvenido!

1. Haz un fork.
2. Añade tu análisis, código o comparación.
3. Abre un pull request.

---

## 📄 Licencia

Este proyecto está bajo la licencia **MIT**.  
Consulta el archivo `LICENSE` para más detalles.
