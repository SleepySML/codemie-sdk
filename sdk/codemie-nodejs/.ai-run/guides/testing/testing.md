# Node SDK Testing

**Project**: codemie-sdk · **Stack**: TypeScript 5.8.3, jest 29.7.0 + ts-jest 29.4.0 · **Scope**: how `sdk/codemie-nodejs` unit tests mock the HTTP layer and when e2e specs run; the test gate commands are in `.ai-run/guides/quality-gates.md`.

## Test shape

### Rules
- R1. In a service unit test, replace the HTTP module with `jest.mock("../../../src/utils/http", () => ({ ApiRequestHandler: jest.fn() }))` and hand the service a mocked handler; only `tests/unit/utils/http.test.ts` and `tests/unit/client/client.test.ts` mock `axios` itself. Evidence: `sdk/codemie-nodejs/tests/unit/services/llm.test.ts::LLMService`
- R2. Run `tests/e2e` specs only against a live CodeMie with `.env.e2e` filled from `.env.e2e.example`; CI runs the unit config only, so an e2e spec is never checked on a pull request. Evidence: `sdk/codemie-nodejs/jest.e2e.config.mjs::setupFilesAfterEnv`
