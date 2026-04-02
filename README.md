# JotDown — REST API Backend

REST API backend for a note-taking application. Built with Node.js, Express, and MongoDB, with a focus on auth architecture, data integrity, and production-level testing practices.

---

## Architecture

```
Request → Route → Auth Middleware → Controller → Model → MongoDB
                       ↓
              Validation Middleware
                       ↓
              Error Handler / Logger
```

- Auth is enforced at the middleware layer — controllers assume a verified user context
- Input validation runs before any database operation
- Centralised error handling returns consistent HTTP responses across all routes

---

## Auth Flow

1. Registration / login → credentials validated → JWT signed and issued
2. JWT stored in HTTP cookie — persists across page refreshes and browser restarts
3. Protected routes pass through auth middleware — invalid or missing token → 401
4. Logout clears the cookie server-side

---

## Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register a new user |
| POST | `/api/auth/login` | Login — sets JWT cookie |
| POST | `/api/auth/logout` | Clears session cookie |

### Notes *(auth required)*
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/notes` | Fetch all notes for authenticated user |
| POST | `/api/notes` | Create a note |
| PUT | `/api/notes/:id` | Update a note |
| DELETE | `/api/notes/:id` | Delete a note |

---

## Tech Stack

- **Node.js / Express.js** — server and routing
- **MongoDB** — document store with structured schemas
- **JWT** — stateless authentication
- **Postman** — endpoint and edge case validation

---

## Project Structure

```
backend/
├── controllers/    # Request handling logic
├── middleware/     # Auth enforcement, input validation, error handling
├── models/         # MongoDB schemas
├── routes/         # Express route definitions
├── tests/          # Unit tests
├── utils/          # Logger and shared utilities
└── server.js       # Entry point
```

---

## Testing

- Unit tests cover core business logic and known edge cases
- Postman collections validate all endpoints against invalid inputs, missing fields, expired tokens, and unauthorised access
