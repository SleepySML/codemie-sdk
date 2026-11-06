# AGENTS.md

Primary AI entrypoint for `codemie-sdk`. Load the relevant `.ai-run/guides/` file before changing code.

## Guide Imports

<!-- ai-run-init:guide-imports start -->
| Category | Guide Path | Purpose |
|---|---|---|
| Project | .ai-run/guides/project.md | Project identity, tracker and source control |
| Quality | .ai-run/guides/quality-gates.md | Quality gates and how to run them |
| Architecture | sdk/codemie-python/.ai-run/guides/architecture/architecture.md | How a Python service is built, registered and wired to the HTTP handler |
| Data | sdk/codemie-python/.ai-run/guides/data/models.md | Request and response model conventions, aliases and datasource routing |
| Development | sdk/codemie-python/.ai-run/guides/development/errors.md | What the Python SDK raises and how a method maps a status to a result |
| Security | sdk/codemie-python/.ai-run/guides/security/auth.md | Authentication modes, localhost handling and SSL verification |
| Testing | sdk/codemie-python/.ai-run/guides/testing/testing.md | How Python unit tests isolate HTTP and which Python versions they must pass |
| Workflows | sdk/codemie-python/.ai-run/guides/workflows/workflow-builder.md | Immutability and validation rules of the workflow builder |
<!-- ai-run-init:guide-imports end -->

## Task Classifier

<!-- ai-run-init:task-classifier start -->
| Category | User Intent | Example Requests | P0 Guide | P1 Guide |
|---|---|---|---|---|
| Architecture | add or change a Python SDK service | "add a service for a new endpoint group", "add a method to an existing service" | sdk/codemie-python/.ai-run/guides/architecture/architecture.md | - |
| Data | add or change a pydantic model or datasource type | "add a datasource type", "add a camelCase field" | sdk/codemie-python/.ai-run/guides/data/models.md | - |
| Development | handle or document an API error in the Python SDK | "treat 404 as already deleted", "write a Raises section" | - | sdk/codemie-python/.ai-run/guides/development/errors.md |
| Security | change authentication or token handling | "add a grant type", "change localhost detection" | sdk/codemie-python/.ai-run/guides/security/auth.md | - |
| Testing | write or fix a Python SDK test | "test a new service method", "mock a failed request" | sdk/codemie-python/.ai-run/guides/testing/testing.md | - |
| Workflows | change the workflow builder or its definition models | "add a builder method", "add a node type" | sdk/codemie-python/.ai-run/guides/workflows/workflow-builder.md | - |
<!-- ai-run-init:task-classifier end -->

## Critical Rules

<!-- ai-run-init:critical-rules start -->
| Rule | Trigger | Action |
|---|---|---|
| Check Guides First | ANY task | Match request to category and load the P0 guide before searching broadly |
| Testing | "write tests" / "run tests" | ONLY then |
| Git Operations | "commit" / "push" / "PR" / "MR" | Only then; load `.ai-run/guides/standards/git-workflow.md` |
| Shell | ANY shell command | Use bash/Linux-compatible syntax |
| Before porting a change between the Python and Node SDKs, load the SDK parity guide | a change touches both sdk/codemie-python and sdk/codemie-nodejs, or ports a feature from one to the other | load `.ai-run/guides/architecture/sdk-parity.md` |
| Keep ticket keys, internal hostnames, credentials and personal e-mail addresses out of commits, PR text, code and tests; use placeholders such as `https://your-codemie-host.example.com`. | writing a commit, PR text, example, fixture or test data | load `.ai-run/guides/standards/git-workflow.md` |
<!-- ai-run-init:critical-rules end -->

## Commands

<!-- ai-run-init:commands start -->
| Need | Source Guide | Source Evidence | Notes |
|---|---|---|---|
| lint-node | .ai-run/guides/quality-gates.md | sdk/codemie-nodejs/package.json::lint | Load guide before running |
| lint-python | .ai-run/guides/quality-gates.md | .github/workflows/ci.yml::jobs.python | Load guide before running |
| format-python | .ai-run/guides/quality-gates.md | .github/workflows/ci.yml::jobs.python | Load guide before running |
| test-python | .ai-run/guides/quality-gates.md | sdk/codemie-python/Makefile::test | Load guide before running |
| test-node | .ai-run/guides/quality-gates.md | sdk/codemie-nodejs/package.json::test | Load guide before running |
| Git / review workflow | .ai-run/guides/standards/git-workflow.md | git history and project context | Load guide before git operations |
<!-- ai-run-init:commands end -->
