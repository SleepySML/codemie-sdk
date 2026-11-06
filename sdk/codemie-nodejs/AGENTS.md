# AGENTS.md

Primary AI entrypoint for `codemie-sdk`. Load the relevant `.ai-run/guides/` file before changing code.

## Guide Imports

<!-- ai-run-init:guide-imports start -->
| Category | Guide Path | Purpose |
|---|---|---|
| Project | .ai-run/guides/project.md | Project identity, tracker and source control |
| Quality | .ai-run/guides/quality-gates.md | Quality gates and how to run them |
| Architecture | sdk/codemie-nodejs/.ai-run/guides/architecture/architecture.md | How a Node service is built, wired into the client and exported |
| Data | sdk/codemie-nodejs/.ai-run/guides/data/mappers.md | Response mappers and datasource type wiring |
| Development | sdk/codemie-nodejs/.ai-run/guides/development/errors.md | Which errors the Node SDK throws and what the HTTP handler does not do |
| Security | sdk/codemie-nodejs/.ai-run/guides/security/auth.md | Auth precedence and SSL verification in the Node client |
| Testing | sdk/codemie-nodejs/.ai-run/guides/testing/testing.md | How Node unit tests mock the HTTP layer and when e2e specs run |
<!-- ai-run-init:guide-imports end -->

## Task Classifier

<!-- ai-run-init:task-classifier start -->
| Category | User Intent | Example Requests | P0 Guide | P1 Guide |
|---|---|---|---|---|
| Architecture | add or change a Node SDK service | "add a service", "add a method with a params schema" | sdk/codemie-nodejs/.ai-run/guides/architecture/architecture.md | - |
| Data | add a mapper or a datasource type in the Node SDK | "map a new response", "add a datasource type" | sdk/codemie-nodejs/.ai-run/guides/data/mappers.md | - |
| Development | handle an API error or add an HTTP verb in the Node SDK | "handle a 404", "call a PATCH endpoint" | - | sdk/codemie-nodejs/.ai-run/guides/development/errors.md |
| Security | change authentication in the Node SDK | "add an auth mode", "change cookie handling" | sdk/codemie-nodejs/.ai-run/guides/security/auth.md | - |
| Testing | write or fix a Node SDK test | "test a service method", "add an e2e spec" | sdk/codemie-nodejs/.ai-run/guides/testing/testing.md | - |
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
