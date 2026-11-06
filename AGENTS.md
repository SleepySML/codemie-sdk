# AGENTS.md

Primary AI entrypoint for `codemie-sdk`. Load the relevant `.ai-run/guides/` file before changing code.

## Guide Imports

<!-- ai-run-init:guide-imports start -->
| Module | Entrypoint | Guides |
|---|---|---|
| sdk/codemie-nodejs | sdk/codemie-nodejs/AGENTS.md | sdk/codemie-nodejs/.ai-run/guides/ |
| sdk/codemie-python | sdk/codemie-python/AGENTS.md | sdk/codemie-python/.ai-run/guides/ |

| Category | Guide Path | Purpose |
|---|---|---|
| Project | .ai-run/guides/project.md | Project identity, tracker and source control |
| Quality | .ai-run/guides/quality-gates.md | Quality gates and how to run them |
| Architecture | .ai-run/guides/architecture/sdk-parity.md | What differs between the Python and Node SDKs when porting a feature |
| Standards | .ai-run/guides/standards/git-workflow.md | PR title, branch, review and tag conventions |
<!-- ai-run-init:guide-imports end -->

## Task Classifier

<!-- ai-run-init:task-classifier start -->
| Category | User Intent | Example Requests | P0 Guide | P1 Guide |
|---|---|---|---|---|
| Architecture | port a change between SDKs or touch both SDKs | "add the same endpoint to both SDKs", "port a Python service method to Node" | .ai-run/guides/architecture/sdk-parity.md | - |
| Standards | commit, branch, open a pull request or tag a release | "write a PR title", "name a branch" | .ai-run/guides/standards/git-workflow.md | - |
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
