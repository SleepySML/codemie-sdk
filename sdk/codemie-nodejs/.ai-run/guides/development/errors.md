# Node SDK Errors

**Project**: codemie-sdk · **Stack**: TypeScript 5.8.3, axios 1.15.0, zod 4.3.5, form-data 4.0.3 · **Scope**: which errors `sdk/codemie-nodejs` throws and what its HTTP handler does not do; auth behaviour is in `security/auth.md`.

## Error handling

### Rules
- R1. Catch `ApiError` and `NotFoundError` from `src/exceptions`: the response interceptor `processFailedResponse` turns each failed axios request (status 400 or above) into one of them, and a failure with no response into `ApiError` with status 500. Evidence: `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler.processFailedResponse`
- R2. Branch on `error.statusCode === 404`, not on the message or the resource: a 404 always becomes `NotFoundError("Resource", "unknown")` and drops the response body, and a 422 joins `error.message`, `help` and `details` into the message. Evidence: `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler.processFailedResponse`
- R3. Expect a plain `Error`, not a `CodeMieError`, from token acquisition: `KeycloakCredentials.getToken` catches each axios failure and throws `Error("Cannot connect to Keycloak at <url>: <reason>")`. Evidence: `sdk/codemie-nodejs/src/auth/credentials.ts::KeycloakCredentials.getToken`

### Constraints
| Do not | Do instead | Why | Evidence |
|---|---|---|---|
| Call a PATCH endpoint through a missing `api.patch` | Add the verb to `ApiRequestHandler` and to the method union of `request` first | the handler exposes `get`, `post`, `put`, `delete`, `postMultipart` and `stream` only | `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler.request` |
| Expect `ApiRequestHandler` to unwrap a `data` envelope | Read `response.data` from a `PaginatedResponse` yourself, as `AssistantService.list` does | `request` returns the axios body unchanged | `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler.request` |
