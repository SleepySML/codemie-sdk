# Node SDK Authentication

**Project**: codemie-sdk · **Stack**: TypeScript 5.8.3, axios 1.15.0, zod 4.3.5, form-data 4.0.3 · **Scope**: auth precedence and SSL verification in the `sdk/codemie-nodejs` client; token-acquisition errors are in `development/errors.md`.

## Authentication

### Rules
- R1. Pass `verify_ssl` on as `rejectUnauthorized` to each `https.Agent`; `ApiRequestHandler` and `KeycloakCredentials`, the only two `new Agent` sites in `src/`, each build their own from it. Evidence: `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler`
- R2. Keep the auth precedence in `CodeMieClient`: `jwt_token` first, then non-empty `cookies`, then Keycloak. With cookies set, `ApiRequestHandler` sends `Cookie` and no static `Authorization`, and only a `jwt_token` adds `Authorization: Bearer` per request through `tokenGetter`. Evidence: `sdk/codemie-nodejs/src/client/client.ts::CodeMieClient`
