# c/v0.26.2 Branching Policy

This document defines the branch policy for the `c/v0.26.2` line in this public fork.

## Branch Roles

`main` tracks the latest RAGFlow code used for research and upstream comparison.

`c/v0.26.2` is based on the upstream `infiniflow/ragflow` `v0.26.2` release tag. It is a public-safe baseline for compatibility work against environments that currently target RAGFlow v0.26.2.

Future internal deployment work should happen in a private GitLab repository. When that repository is available, create an internal-only branch from this baseline, for example `c/v0.26.2-internal`.

## Public-Safe Scope

The `c/v0.26.2` branch may contain:

- Public upstream code from RAGFlow.
- Generic bug fixes that do not reveal private infrastructure.
- Public-safe compatibility notes.
- Documentation that explains workflow, branching, and migration policy.
- Example configuration templates without real values.

The `c/v0.26.2` branch must not contain:

- Real `.env` files or secrets.
- API keys, tokens, certificates, private keys, or passwords.
- Internal hostnames, IP addresses, domains, or service URLs.
- Private Docker registry addresses.
- Company-specific business logic, customer data, or sample data.
- Deployment files that reveal internal infrastructure.

## Internal Branch Policy

After the private GitLab repository is created, use this model:

```text
main
  Public fork research line, tracks latest upstream code.

c/v0.26.2
  Public-safe v0.26.2 baseline.

c/v0.26.2-internal
  Private GitLab-only line for internal configuration, deployment, and microservice adaptation.
```

Only push `c/v0.26.2-internal` to the private GitLab remote. Do not push that branch to the public GitHub fork.

## Suggested Remote Layout

```text
origin
  Public GitHub fork.

upstream
  Official RAGFlow repository: https://github.com/infiniflow/ragflow.git

gitlab
  Future private company GitLab repository.
```

## Commit Rules

Before committing to `c/v0.26.2`, check that the change is safe to publish publicly.

Use clear commit prefixes when practical:

```text
docs: document v0.26.2 branch policy
fix: adjust public-safe compatibility behavior
chore: sync v0.26.2 baseline metadata
```

For internal GitLab-only commits, prefer prefixes that make deployment intent clear:

```text
deploy: add internal compose profile
config: wire internal service defaults
ops: add private registry deployment notes
```

## Migration To GitLab

When the private GitLab repository is ready:

```powershell
git remote add gitlab <private-gitlab-ragflow-url>
git push gitlab c/v0.26.2
git switch -c c/v0.26.2-internal c/v0.26.2
git push -u gitlab c/v0.26.2-internal
```

After that point, keep internal configuration and deployment work on the GitLab-only branch.
