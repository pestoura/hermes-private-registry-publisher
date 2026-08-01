## Purpose

Describe the exact private-publisher change and its authorization gate.

## Scope

List every changed file and confirm whether the PR contains executable workflow logic.

## Security checklist

- [ ] No credential, token, Docker authentication data or authorization header is included.
- [ ] No self-hosted runner or Hermes Docker socket is used.
- [ ] No automatic deployment to Hermes is introduced.
- [ ] Third-party Actions, when present, are pinned by full commit SHA.
- [ ] Workflow permissions, when present, are explicitly minimal.
- [ ] Publication uses this repository's `GITHUB_TOKEN`, not a personal token.
- [ ] Source, dependencies and runtime references are immutable.
- [ ] Package visibility and access implications are documented.
- [ ] Public rollback digest remains unchanged.

## Authorization boundary

State explicitly which action this PR authorizes and which later actions remain blocked.

## Validation

Record the exact commit reviewed, checks executed and sanitized evidence references.
