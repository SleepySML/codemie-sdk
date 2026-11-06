# Python SDK Error Handling

**Project**: codemie-sdk · **Stack**: Python requests ^2.31.0, pydantic ^2.12.2 · **Scope**: what the Python SDK raises and how a service method maps an HTTP status or body to a result; service wiring is in `architecture/architecture.md`.

## Error handling

### Rules
- R1. Expect `requests.HTTPError` for a non-2xx status: each `ApiRequestHandler` verb calls `response.raise_for_status()` (`post` only while `raise_on_error=True`), and nothing in `src/` raises `ApiError`. Evidence: `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler.post`
- R2. Name `requests.HTTPError` with its status codes in a method's `Raises:` section, not `ApiError` (the HTTP layer never raises `ApiError`). Known debt, to fix: `sdk/codemie-python/src/codemie_sdk/services/categories.py`, `sdk/codemie-python/src/codemie_sdk/services/files.py`, `sdk/codemie-python/src/codemie_sdk/services/task.py`, `sdk/codemie-python/src/codemie_sdk/services/mermaid.py`, `sdk/codemie-python/src/codemie_sdk/services/datasource.py`, `sdk/codemie-python/src/codemie_sdk/services/workflow.py`, `sdk/codemie-python/src/codemie_sdk/services/integration.py`. Evidence: `sdk/codemie-python/src/codemie_sdk/services/usage_attribution.py::UsageAttributionService`
- R3. To turn a POST status into a result, request `requests.Response` with `raise_on_error=False`, branch on `response.status_code`, then call `response.raise_for_status()` for every other status (`raise_on_error` exists on `post` only). Evidence: `sdk/codemie-python/src/codemie_sdk/services/dynamic_config.py::DynamicConfigService.set`
- R4. To accept a status on GET, PUT, PATCH or DELETE, catch `requests.exceptions.HTTPError`, test `exc.response.status_code` and re-raise every other status (those verbs always raise). Evidence: `sdk/codemie-python/src/codemie_sdk/services/dynamic_config.py::DynamicConfigService.delete`
- R5. Raise `NotFoundError(resource_type, id)` only for a lookup the SDK does client-side; today only `IntegrationService.get` and `get_by_alias` do, and they search the first 100 results of `list` (`per_page=100`), so an integration past them raises `NotFoundError` although it exists. Evidence: `sdk/codemie-python/src/codemie_sdk/services/integration.py::IntegrationService.get`

### Constraints
| Do not | Do instead | Why | Evidence |
|---|---|---|---|
| Rely on the default `wrap_response=True` for a body that is the model itself and has a top-level `data` field | Pass `wrap_response=False` | `_parse_response` returns `body["data"]` for any dict body holding that key | `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler._parse_response` |
| Type a method `-> Model` when its endpoint can answer 204 or an empty body | Type it `-> Model | None`, or request `dict` | `_parse_response` returns `None` for an empty or 204 body, and `{}` when the model is `dict` | `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler._parse_response` |
