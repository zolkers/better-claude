# Node.js

Inherits from `_shared/conventions.md` and `javascript/javascript.md` or `typescript/typescript.md` depending on the project.

## Detection

This guide applies when `package.json` has a `"main"` or `"bin"` field, or `node_modules/` exists with server-side dependencies (express, fastify, koa, etc.).

## Conventions

### Project Setup

- Use `"type": "module"` in `package.json` for ESM by default.
- Use `dotenv` or `node --env-file` (20.6+) for environment variables -- never hardcode.
- Use `engines` field in `package.json` to lock Node version.
- Always have a `.nvmrc` or `.node-version` file.

### API Design

- Use a framework: Express, Fastify, Hono, or Koa -- never raw `http.createServer` for APIs.
- Separate route definitions from business logic -- routes call services, services call repositories.
- Validate all incoming data at the boundary (Zod, Joi, AJK, class-validator).
- Return consistent error responses with status code, message, and error code.
- Use async route handlers with proper error catching (express-async-errors or fastify's native async support).

### Error Handling

- Use a centralized error handler middleware -- don't catch errors in every route.
- Define custom error classes: `NotFoundError`, `ValidationError`, `UnauthorizedError`.
- Never expose stack traces in production -- log them, return safe messages.
- Handle unhandled rejections and uncaught exceptions at process level with graceful shutdown.

```javascript
process.on("unhandledRejection", (reason) => {
  logger.fatal("Unhandled rejection", { reason });
  process.exit(1);
});
```

### Security

- Use `helmet` for HTTP security headers.
- Use `cors` with explicit origin configuration.
- Rate limit all public endpoints.
- Sanitize inputs -- never pass user input directly to database queries, file paths, or shell commands.
- Use parameterized queries -- never string concatenation for SQL.
- Keep dependencies updated -- use `npm audit` regularly.

### Database

- Use an ORM (Prisma, Drizzle, TypeORM) or query builder (Knex) -- avoid raw queries unless performance-critical.
- Use migrations for schema changes -- never modify production schemas manually.
- Use connection pooling.
- Always handle transactions for multi-step operations.

### Logging

- Use a structured logger: pino, winston -- never `console.log` in production.
- Log with levels: `error`, `warn`, `info`, `debug`.
- Include request ID / correlation ID in all logs.
- Don't log sensitive data (passwords, tokens, PII).

### Process Management

- Use graceful shutdown: close server, finish pending requests, close DB connections.
- Use health check endpoints (`/health`, `/ready`).
- Use `cluster` module or a process manager (PM2) for production multi-core usage.

### Testing

- Use `supertest` for HTTP endpoint testing.
- Use `testcontainers` or in-memory DB for integration tests.
- Mock external APIs with `msw` (Mock Service Worker) or `nock`.

## Project Structure

```
src/
├── routes/              # Route definitions
│   ├── user.routes.ts
│   └── auth.routes.ts
├── controllers/         # Request/response handling
│   ├── user.controller.ts
│   └── auth.controller.ts
├── services/            # Business logic
│   ├── user.service.ts
│   └── auth.service.ts
├── repositories/        # Data access
│   └── user.repository.ts
├── middleware/           # Express/Fastify middleware
│   ├── auth.middleware.ts
│   ├── error.middleware.ts
│   └── validate.middleware.ts
├── models/              # DB models / schemas
├── utils/               # Helpers
├── config/              # App configuration
│   └── index.ts
├── types/               # Shared types
└── app.ts               # App setup (no listen)
```

## Questions to Ask

- Framework: Express, Fastify, Hono, Koa?
- Database: PostgreSQL, MySQL, MongoDB, SQLite?
- ORM: Prisma, Drizzle, TypeORM, Knex?
- Auth: JWT, session-based, OAuth?
- Using TypeScript or plain JavaScript?
- Deployment target: Docker, serverless, VPS?
