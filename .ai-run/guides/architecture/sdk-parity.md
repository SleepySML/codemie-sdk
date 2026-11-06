# SDK Parity Guide

**Project**: codemie-sdk · **Stack**: none (docs, GitHub Actions) · **Scope**: what differs between the Python and Node SDKs when porting a feature or changing both; patterns inside one SDK live in `sdk/codemie-python/.ai-run/guides/` and `sdk/codemie-nodejs/.ai-run/guides/`

## Structure and boundaries

### Rules
- R1. Before porting a feature or writing an example for the other SDK, check that SDK's `CodeMieClient`: the Node client wires a subset of the Python services, with no `admin`, `projects`, `mcp_configs`, `webhook`, `usage_attribution` or `vendor_*` service (a port can need a new service, not just a method). Evidence: `sdk/codemie-nodejs/src/client/client.ts::CodeMieClient`
- R2. Write error handling per SDK, never copied across: Python surfaces a non-2xx status as `requests.HTTPError` from `ApiRequestHandler` (`raise_for_status()`), Node surfaces it as `ApiError`/`NotFoundError` from the axios interceptor `processFailedResponse`. Evidence: `sdk/codemie-nodejs/src/utils/http.ts::ApiRequestHandler.processFailedResponse`
- R3. Unwrap `data` yourself in Node: Python `_parse_response` returns `body["data"]` for a dict body by default (`wrap_response=True`), while Node `ApiRequestHandler.request` returns the axios body unchanged, so a ported method reads `response.data` explicitly. Evidence: `sdk/codemie-python/src/codemie_sdk/utils/http.py::ApiRequestHandler._parse_response`

### Read first
| When you are | Exemplar | Why this one | Tests |
|---|---|---|---|
| porting a datasource type between SDKs | `sdk/codemie-python/src/codemie_sdk/services/datasource.py::_create_endpoint` | Python keeps an explicit `_ROUTES` table and raises `ValueError` for an unrouted type; Node maps through `DATA_SOURCE_TO_PATH_PARAM` | `sdk/codemie-python/tests/test_datasource_routes.py::test_types_without_route_raise_before_http` |
