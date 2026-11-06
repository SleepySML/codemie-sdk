# Quality Gates

Run order is fastest-to-slowest.

## Full runs

| Command | Covers | Source |
|---|---|---|
| `CI workflow ci.yml job python (Python 3.13): poetry install, ruff check, ruff format --check, pytest` | Python lint, format check, unit tests | .github/workflows/ci.yml::jobs.python |
| `CI workflow ci.yml Node job: npm install, npm run lint, npm test, npm run build` | Node lint, build, unit tests | .github/workflows/ci.yml::jobs |
| `CI-only: secrets-detection, internal-refs (ci.yml) and CodeQL (codeql.yml) via external codemie-ai/ci-workflows reusable workflows` | secret scan, internal-reference scan, static analysis (CI only, no local command) | .github/workflows/ci.yml, .github/workflows/codeql.yml |

---

### lint-node

**Run**: `cd sdk/codemie-nodejs && npm run lint`
**Pass**: exit code 0
**Fail**: non-zero exit code on any lint error or warning

### lint-python

**Run**: `cd sdk/codemie-python && poetry run ruff check .`
**Pass**: exit code 0
**Fail**: non-zero exit code
**Auto-fix**: cd sdk/codemie-python && poetry run ruff check --fix .

### format-python

**Run**: `cd sdk/codemie-python && poetry run ruff format --check .`
**Pass**: exit code 0
**Fail**: non-zero exit code
**Auto-fix**: cd sdk/codemie-python && make ruff-format

### test-python

**Run**: `cd sdk/codemie-python && make test`
**Pass**: exit code 0
**Fail**: non-zero exit code

### test-node

**Run**: `cd sdk/codemie-nodejs && npm test`
**Pass**: exit code 0
**Fail**: non-zero exit code
