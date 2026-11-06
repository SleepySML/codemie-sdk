# Python SDK Testing

**Project**: codemie-sdk · **Stack**: Python pytest, requests ^2.31.0, pydantic ^2.12.2 · **Scope**: how Python SDK unit tests isolate HTTP and which Python versions they must pass; the gate commands are in `.ai-run/guides/quality-gates.md`.

## Running tests

### Rules
- R1. Keep unit tests off the network: patch the verb on `codemie_sdk.utils.http.requests` (`patch("codemie_sdk.utils.http.requests.get")`) and return a `MagicMock` response, or assign a fake object to `service._api`. Evidence: `sdk/codemie-python/tests/test_mcp_configs_service.py::TestList`
- R2. Keep code and tests compatible with Python 3.12: the release `verify` job runs 3.12 and skips the format check, while PR CI runs 3.13 with it, so a 3.13-only construct passes the PR and fails the release. Evidence: `.github/workflows/publish-python.yml::jobs.verify`
