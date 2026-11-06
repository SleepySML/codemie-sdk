# Python SDK Workflow Builder

**Project**: codemie-sdk · **Stack**: Python requests ^2.31.0, pydantic ^2.12.2 · **Scope**: immutability, validation and publishing rules of `WorkflowBuilder` and its definition models; the HTTP service layer is in `architecture/architecture.md`.

## Patterns in use

### Rules
- R1. Return a new builder from each `WorkflowBuilder` method through `_replaced` or a `_with_*` helper, and never assign `self._state` outside `__init__` (callers branch one builder into independent chains). Evidence: `sdk/codemie-python/src/codemie_sdk/builders/workflow/builder.py::WorkflowBuilder._replaced`
- R2. Declare a new workflow definition model with `model_config = _MIRRORED_MODEL_CONFIG` (frozen, `extra="forbid"`): instances are shared across builder branches and `render()` calls, and a mistyped kwarg must fail at construction instead of changing the workflow. Evidence: `sdk/codemie-python/src/codemie_sdk/builders/workflow/definition.py::_MIRRORED_MODEL_CONFIG`
- R3. Let `requests.HTTPError` from `WorkflowService.create_workflow` or `update` propagate out of `publish` unchanged; resolve assistant names before the HTTP call and raise `ValueError` on zero or several matches. Evidence: `sdk/codemie-python/src/codemie_sdk/builders/workflow/publish.py::_publish`

### Read first
| When you are | Exemplar | Why this one | Tests |
|---|---|---|---|
| adding a builder method for a new state or node kind | `sdk/codemie-python/src/codemie_sdk/builders/workflow/builder.py::WorkflowBuilder.add_tool_state` | checks the referenced id, builds the state model, returns through `_with_state` | `sdk/codemie-python/tests/test_workflow_nodes.py::TestToolNode` |
