# Node SDK Architecture

**Project**: codemie-sdk · **Stack**: TypeScript 5.8.3, axios 1.15.0, zod 4.3.5, form-data 4.0.3 · **Scope**: how a service in `sdk/codemie-nodejs` is built, wired into `CodeMieClient` and exported; errors are in `development/errors.md`, mappers in `data/mappers.md`, auth in `security/auth.md`.

## Extension points

### Rules
- R1. Wire a new service in three places in `CodeMieClient`: the private field assigned in the constructor, the same assignment in `refreshServices`, and a getter (`refreshServices` rebuilds the listed services after `initialize`, `getToken` and `refreshToken`, so a service missing there keeps the constructor's `AuthConfig`). Evidence: `sdk/codemie-nodejs/src/client/client.ts::CodeMieClient.refreshServices`
- R2. Export a new service, model or schema module from its barrel (`src/services/index.ts`, `src/models/index.ts`, `src/schemas/index.ts`): the package entry `src/index.ts` re-exports only those barrels, the client, the exceptions and `formatCookies`. Known debt, to fix: `sdk/codemie-nodejs/src/models/project.ts`. Evidence: `sdk/codemie-nodejs/src/index.ts::services`
- R3. Build a top-level service on `new ApiRequestHandler(config)` from the `AuthConfig` it receives, and give a sub-resource service the parent's handler instead, as `WorkflowService.executions` does with `WorkflowExecutionService`. Evidence: `sdk/codemie-nodejs/src/services/workflow.ts::WorkflowService.executions`
- R4. Parse a params object with its zod schema from `src/schemas/<resource>.ts` at the top of the method (`const params = XParamsSchema.parse(_params)`) before sending it, as `AssistantService.list` does; responses are typed with interfaces from `src/models/` and not validated at runtime. Evidence: `sdk/codemie-nodejs/src/services/assistant.ts::AssistantService.list`
