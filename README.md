# 📋 Comparing REST API Designs for a To-Do App: ChatGPT vs Claude vs DeepSeek

> How would **ChatGPT**, **Claude**, and **DeepSeek** design a REST API for a web To-Do application?  
> This repository compares three AI-generated API designs, analyzing their structure, clarity, technical depth, and readiness for real-world implementation.

---

## 📂 Repository Structure

```
Designing-an-API-with-LLMs/
├── api-chatgpt.md     # API design by ChatGPT
├── api-claude.md      # API design by Claude
├── api-deepseek.md    # API design by DeepSeek
└── README.md          # This file: comparison and analysis
```

All three designs aim to define a **REST API for a To-Do List web application**, but each reflects the unique style, priorities, and architectural tendencies of its generating model.

---

## 🎯 Project Objective

This project explores how three leading large language models — **ChatGPT**, **Claude**, and **DeepSeek** — respond to the same prompt:  
*"Design a REST API for a web-based To-Do List application."*

We analyze:
- Adherence to REST principles.
- Clarity and usability of documentation.
- Architectural depth and scalability.
- Developer experience and production-readiness.

The goal is not to declare a "winner", but to **highlight different philosophies in API design** and help developers understand how AI models approach real-world software tasks.

---

## 🔍 High-Level Comparison

| Feature | **ChatGPT** | **Claude** | **DeepSeek** |
|--------|-------------|------------|--------------|
| **Design Style** | Educational, architecture-first (DDD, Clean Architecture) | Minimalist, clean, focused on core functionality | Feature-rich, practical, user-centric |
| **Task Model** | `title`, `description`, `completed`, `dueDate` | `title`, `description`, `completed`, `dueDate` | `title`, `description`, `dueDate`, `priority`, `status` (pending/completed) |
| **Authentication** | JWT with `/auth/register` and `/auth/login` | Same | Same |
| **Mark as Complete** | `PATCH /tasks/{id}` with `{"completed": true}` | `PATCH /tasks/{id}` with `{"completed": true}` | ✅ Dedicated `PATCH /tasks/{id}/complete` and `PATCH /tasks/{id}/pending` |
| **Categories** | Mentioned as optional | ❌ Not included | ✅ Full CRUD: `GET /categories`, `POST /categories`, and assignment |
| **Response Style** | Returns full resource (RESTful) | Returns full resource | Uses `{ "message": "..." }` (less RESTful) |
| **HTTP Status Codes** | Correct (`201`, `204`) | Correct | Uses `200` for `DELETE` (should be `204`) |
| **Request Examples** | JSON examples | JSON examples | ✅ Includes full `curl` commands with headers and JWT |
| **Advanced Features** | ✅ DDD, DTOs, layered project structure | Minimalist, no extra features | ✅ Pagination, search, sorting, rate limiting suggestions |
| **Production Readiness** | Ideal for scalable, team-based apps | Simple and clean | Fast MVP, rich feature set |

> 💡 **Summary**:
> - **ChatGPT**: Best for **architectural clarity** and long-term maintainability.
> - **Claude**: Most **minimalist and consistent**, but lacks advanced features.
> - **DeepSeek**: Most **feature-complete and practical**, ideal for rapid development.

---

## 🛠️ Key Functionalities Across All Designs

### ✅ Common Core Features
- Create, read, update, delete tasks (`POST`, `GET`, `PUT`, `DELETE`).
- User authentication with JWT.
- Input validation and error handling.
- Filtering tasks by status, date, etc.

### ✅ Unique Strengths

#### **ChatGPT**
- **Clean Architecture & DDD**: Clearly separates domain, application, infrastructure, and interfaces.
- **Project structure**: Suggests a full folder layout for maintainability.
- **Best REST practices**: Returns full resources, uses correct status codes (`204 No Content` on delete).

#### **Claude**
- **Simplicity and consistency**: Clean, minimal, no distractions.
- **Standard REST approach**: Predictable and easy to follow.
- **No unnecessary complexity** — ideal for learning or small projects.

#### **DeepSeek**
- **Rich task model**: Includes `priority` and `status` (not just boolean `completed`).
- **Dedicated endpoints**: `PATCH /tasks/{id}/complete` improves UX and clarity.
- **Categories support**: Full CRUD for organizing tasks.
- **Practical examples**: Full `curl` commands make integration easier.
- **Production tips**: Explicitly suggests pagination, search, sorting, and rate limiting.

---

## 🧠 Style & Philosophy Analysis

| Aspect | ChatGPT | Claude | DeepSeek |
|------|-------|--------|--------|
| **Clarity** | High, with explanations | High, concise | High, but response format less RESTful |
| **Technical Depth** | Strong focus on architecture | Moderate | Focused on functionality |
| **Scalability** | Excellent — built for growth | Good | Good, but lacks modular structure |
| **Developer Experience** | Ideal for teams and onboarding | Great for beginners | Best for rapid prototyping |

> 🎯 **Use Case Recommendations**:
> - Use **ChatGPT’s design** if you're building a scalable app with clean architecture.
> - Use **Claude’s design** if you want a minimal, clean, and predictable API.
> - Use **DeepSeek’s design** if you need rich features fast (MVP, demo, startup).

---

## 📚 How to Use This Repository

1. **Review all three designs**:
   - Compare how each model structures endpoints, responses, and features.
   - Notice differences in documentation style and completeness.

2. **Choose your inspiration**:
   - Need architecture? → **ChatGPT**
   - Want simplicity? → **Claude**
   - Need features fast? → **DeepSeek**

3. **Combine the best of all worlds**:
   - Use ChatGPT’s structure.
   - Adopt DeepSeek’s dedicated endpoints and categories.
   - Keep Claude’s consistency.

4. **Implement a real API**:
   - Build a backend with FastAPI, Express, or Spring Boot.
   - Add a database (SQLite, PostgreSQL).
   - Connect to a frontend (React, Vue, etc.).

5. **Extend it**:
   - Generate OpenAPI (Swagger) specs.
   - Add tests, CI/CD, or monitoring.

---

## 💡 Ideas to Extend

- ✅ Generate an `openapi.yaml` file from each design.
- ✅ Implement all three APIs and compare development effort.
- ✅ Conduct a developer survey: which design is easier to understand?
- ✅ Build a frontend that works with all three (with adapters).
- ✅ Add authentication, pagination, and search to all versions.

---

## 🤝 Contributions

Want to add a design from another LLM (like Gemini, Mistral, or Llama)?  
Or implement one of the APIs in code?  
Contributions are welcome!

1. Fork the repository.
2. Add your enhancement or comparison.
3. Open a pull request with a clear description.

---

## 📄 License

This project is licensed under the **MIT License**.  
See the `LICENSE` file for details.

---

> 🚀 *Do you prefer clean architecture, minimalist design, or feature-rich functionality?*  
> With this repository, you can see how AI models answer that question — and build a better API as a result.
