# Python SDK Models

**Project**: codemie-sdk · **Stack**: Python requests ^2.31.0, pydantic ^2.12.2 · **Scope**: pydantic request and response model conventions, camelCase aliases and datasource routing; workflow definition models are in `workflows/workflow-builder.md`.

## Data access

### Rules
- R1. Map a backend camelCase key with `Field(alias=...)` plus `populate_by_name=True`, and send the request with `model_dump(by_alias=True, exclude_none=True)` (a dump without `by_alias` sends the Python field name). Evidence: `sdk/codemie-python/src/codemie_sdk/services/datasource.py::DatasourceService.create`
- R2. Give a new `DataSourceType` an entry in `_ROUTES` in `services/datasource.py`, and add it to `_NAME_IN_UPDATE_PATH` when its update URL carries the datasource name (`_create_endpoint` raises `ValueError` for an unrouted type before any request). Evidence: `sdk/codemie-python/src/codemie_sdk/services/datasource.py::_create_endpoint`
- R3. Treat `required_fields()` on a datasource request as any-of: `pre_init_validator` raises only when none of the listed fields is set, so a field that must always be present needs its own validator or a required `Field(...)`. Evidence: `sdk/codemie-python/src/codemie_sdk/models/datasource.py::BaseDataSourceRequest.pre_init_validator`

### Read first
| When you are | Exemplar | Why this one | Tests |
|---|---|---|---|
| adding a datasource request model the backend validates strictly | `sdk/codemie-python/src/codemie_sdk/models/datasource.py::SvnDataSourceRequest` | `extra="forbid"` rejects a mistyped kwarg, camelCase aliases, `required_fields`, fixed `type` in `__init__` | `sdk/codemie-python/tests/test_datasource_new_kinds.py::test_svn_requires_link_and_makes_no_http_call` |
