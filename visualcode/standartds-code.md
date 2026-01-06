### 6. Implementation Standards (Dec 2025 Update)
- **Response envelopes everywhere**: Controllers must return `{ data }`, `{ data, pagination }`, or `{ error }`. List endpoints should manually map raw arrays to `{ items, pagination }` before the global interceptor wraps them, ensuring Swagger and runtime stay aligned.
- **Swagger decorators**: Apply shared decorators from `swagger.decorators.ts` on every route (`@ApiDataResponse`, `@ApiListResponse`, `@ApiErrorResponse`). Keep `@ApiTags('public'|'private'|...)` consistent so the docs surface all surfaces.
- **DTO-backed responses**: Every response shape needs an explicit DTO under its module's `dto/` folder (even simple `{ ok: true }`). Annotate with `@ApiProperty` and reuse wherever possible.
- **Structured logging**: All error paths (API failures, downstream calls, normalization issues) must log via the common logger with context (fingerprints, tenant, endpoint). No silent failures.
- **Naming conventions**: Route paths use kebab-case (`/projects/:id/owner-map`), Prisma columns remain snake_case. Align NestJS files with the agreed module/service/controller layout so everything feels authored by one team.
- **No caches/tests right now**: Skip cache layers and Jest/unit-test scaffolding for the API or MCP servers until explicitly requested. Runtime/business-critical logic may still include inline assertions or guards, but no standalone test suites.
- **Legacy cleanup**: Remove unused/legacy code when encountered instead of leaving dead modules around.
