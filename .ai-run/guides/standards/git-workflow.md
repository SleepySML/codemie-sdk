# Git Workflow

**Project**: codemie-sdk · **Stack**: none (docs, GitHub Actions) · **Scope**: branch names, commit subjects, merge policy and their recovery steps; exact gate commands live in `.ai-run/guides/quality-gates.md`, the MR adapter in `.ai-run/guides/project.md`

## Branch Naming Convention

- base branch: main
- pattern: none enforced; observed `feat/<slug>`, `fix/<slug>`, `ci/<slug>`, `docs/<slug>`

### Rules
- R1. Branch from `main` and open the pull request against `main` (CONTRIBUTING.md:10-13). No branch pattern is enforced: CI and hooks check none. The public branches use `feat/<slug>`, `fix/<slug>`, `ci/<slug>` and `docs/<slug>` (from `git branch -r` on the `fork` remote).

## Commit Message Format

### Rules
- R1. PR titles follow Conventional Commits `<type>(<optional scope>): <description>`, with type one of feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert; scopes seen on main are `python` and `workflows`, and CONTRIBUTING gives `nodejs`; a breaking change adds `!` after the scope. Source: CONTRIBUTING.md:17-18; `.github/workflows/pr-title-lint.yml` lines 1-18 delegate the check to the external `codemie-ai/ci-workflows` reusable-commit-lint workflow, whose body is not in this repository, so the check's exact rules are unverified.
- R2. Individual commit messages are not checked (CONTRIBUTING.md:20). On `github/main` each change lands as one commit whose subject is the PR title followed by `(#<PR number>)`, which matches squash merging; the repository merge settings are not visible from the checkout, so squash is observed, not required.
- R3. Keep ticket keys, internal hostnames, credentials and personal e-mail addresses out of commits, PR text, code and tests; use placeholders such as `https://your-codemie-host.example.com`.
- R4. No local commit hooks exist: the repository has no `.husky/`, `.pre-commit-config.yaml` or commitlint config, so lint, format and tests run only when invoked or in CI.

## Merge Strategy

### Rules
- R1. Merge into `main` by squash (each pull request lands as one commit on `main`). Evidence: recorded user decision
- R2. A pull request needs one approval from a collaborator with write access and every CI check green before merge (CONTRIBUTING.md:22-24); this is the stated convention, as the branch protection settings are not visible from the checkout. The body follows `.github/PULL_REQUEST_TEMPLATE.md`: Summary, Changes, Testing (which SDK), Checklist.
- R3. Never commit a version change for a release: the tag is the version (`python-vX.Y.Z[-rcN]`, `node-vX.Y.Z[-rcN]`) and the publish workflows stamp it into the build only (`publish-python.yml` lines 95-108 run `poetry version "$VERSION"`, `publish-node.yml` lines 91-105 run `npm version --no-git-tag-version`, neither commits). Raise the base `MAJOR.MINOR.0` in `pyproject.toml` or `package.json` only to start a new line; releases go through the `codemie-release` skill.

## Anti-Patterns

| Do not | Do instead | Why |
|---|---|---|
| Cut a branch from another feature branch | `git switch -c feat/<slug> main` | rebases and PR diffs stay against the integration branch |
| Push to `main` directly | open a pull request from your branch | a pull request needs one approval and green CI before merge (CONTRIBUTING.md:22-24) |
| Force-push a shared branch with `--force` | `git push --force-with-lease` after a rebase, and only on your own branch | a teammate's commits survive |

## Troubleshooting

| Issue | Fix |
|---|---|
| Branch was cut from the wrong base | `git rebase --onto main <old-base> <branch>`, then `git push --force-with-lease` |
| Rebase conflicts on `main` | resolve file by file, `git add`, `git rebase --continue`; `git rebase --abort` to restart; never resolve by taking one side wholesale |
