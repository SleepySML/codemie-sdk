# Python SDK Architecture

**Project**: codemie-sdk · **Stack**: Python requests ^2.31.0, pydantic ^2.12.2 · **Scope**: how a Python SDK service is built, registered on `CodeMieClient` and wired to the HTTP handler; error mapping is in `development/errors.md`, models in `data/models.md`.

## Patterns in use

### Rules
- R1. Register a new top-level service as an attribute in `CodeMieClient.__init__`, built as `XService(self._api_domain, _token_source, verify_ssl=...)` (a service not assigned there is not reachable as `client.<name>`). Evidence: `sdk/codemie-python/src/codemie_sdk/client/client.py::CodeMieClient`
- R2. Build the service's own `ApiRequestHandler(api_domain, token, verify_ssl)` in `__init__` and send every request through `self._api` (the handler owns headers, SSL verification, the `data` unwrap and `raise_for_status`). Evidence: `sdk/codemie-python/src/codemie_sdk/services/llm.py::LLMService`
- R3. Pass the token source callable (`_token_source`, `TokenSource = str | Callable[[], str]`), not a resolved token string, so `ApiRequestHandler._get_headers` calls it on every request and picks up the refreshed Keycloak token. Evidence: `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler._get_headers`

### Constraints
| Do not | Do instead | Why | Evidence |
|---|---|---|---|
| Call `requests.get/post/put/patch/delete` from a module in `services/` | Call the matching `self._api` method | no service in `services/` calls `requests` directly; only `auth/credentials.py` does, for Keycloak and local-auth | `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler` |

### Read first
| When you are | Exemplar | Why this one | Tests |
|---|---|---|---|
| adding a service with list, clone and update calls | `sdk/codemie-python/src/codemie_sdk/services/mcp_configs.py::MCPConfigsService` | small, current, one handler, typed request and response models | `sdk/codemie-python/tests/test_mcp_configs_service.py::TestList` |
| adding a sub-resource service under a parent resource | `sdk/codemie-python/src/codemie_sdk/services/workflow.py::WorkflowService.executions` | hands the parent's `self._api` and the parent id to `WorkflowExecutionService` instead of building a second handler | no covering test |
