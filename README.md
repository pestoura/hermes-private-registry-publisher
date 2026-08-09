# Hermes Private Registry Publisher

Publication boundary for reviewed GitHub Actions workflows that create private GHCR packages consumed by Hermes.

## Security boundary

- Public canonical source: `pestoura/hermes-security-labs`.
- Target publisher and package-permission boundary: this repository, **private in the accepted final operating state**.
- Pilot package: `ghcr.io/pestoura/hermes-private-vampi`.
- Publication uses this repository's ephemeral `GITHUB_TOKEN`.
- Hermes runtime access must use a separate credential limited to `read:packages`.
- No self-hosted runner, Hermes Docker socket or automatic deployment is permitted.
- No PAT, Docker auth material, private key or reusable registry credential may be committed here.

## Current state — 2026-08-09

The private VAmPI publication pilot has already executed successfully:

- publisher workflow: `.github/workflows/publish-private-vampi.yml`;
- owner-triggered publication run: `30680647184`;
- publication result: `SUCCESS`;
- publisher commit used for publication: `1ce1b1c72c20cf9267fbdc460f40fcfe1d310d08`;
- private VAmPI OCI index: `sha256:b1b66324a2d35cfe55e3edcd81f9f3c012907c71367df37f83d9ef63b500b3d3`;
- SBOM and BuildKit provenance: enabled by the accepted publication workflow;
- Hermes deployment: not performed by the publisher workflow.

An automated anonymous-deny gate is maintained in `.github/workflows/private-vampi-anonymous-deny.yml`. It uses an isolated empty Docker credential configuration and fails closed unless the exact accepted digest is denied specifically for authentication/authorization reasons.

## Accepted boundary and temporary implementation exception

The accepted **final operating state** keeps this publisher repository `private` and keeps the `hermes-private-vampi` package private.

On 2026-08-09 the owner explicitly authorized a temporary implementation exception: this publisher repository may be changed to `public` solely while repository implementation and GitHub Actions validation are being completed, because GitHub-hosted Actions are blocked for this account while the repository is private.

This exception does **not** authorize any of the following:

- changing `hermes-private-vampi` package visibility from `private`;
- granting `pestoura/hermes-security-labs` or another repository package access;
- storing a PAT, Docker auth material or reusable registry credential in GitHub;
- using the public publisher repository as the Hermes runtime credential boundary;
- treating the temporary public repository state as the accepted final operating state.

Before issue `#53` can be closed, the publisher repository must be restored to `private` and the private-package boundaries must be revalidated.

The owner has separately confirmed the GitHub Package Settings gate for `hermes-private-vampi`: the package remains private, the private publisher is the intended repository access boundary, the public canonical source repository has no package access, and no unapproved repository, team or user access is present.

The linked GitHub integration still receives `403 Resource not accessible by integration` from the Packages API. Therefore package repository-access details are recorded as **owner-confirmed**, not API-verified. This limitation does not override or weaken the independent runtime access controls.

The package itself must remain private throughout. Making the package public would permanently reject that package identity for the private target state.

## Remaining operational gate

With the temporary public-repository implementation exception active, the controlled sequence is:

1. run and merge the repository-only validation/reconciliation work while the publisher is temporarily public;
2. provision a distinct Hermes consumer credential with exactly `read:packages` outside GitHub and outside version control;
3. authenticate through stdin using an isolated Docker configuration;
4. prove exact-digest private pull;
5. prove absence of push/delete authority without package mutation;
6. run temporary private VAmPI lifecycle parity;
7. review and merge the separate Compose migration;
8. perform exact-SHA post-merge acceptance;
9. demonstrate rollback to the accepted public digest;
10. reconcile deployment tracking and drift detection;
11. restore the publisher repository to `private` and revalidate final boundaries before closure.

No consumer PAT is stored in this repository or in GitHub Actions for the runtime acceptance path.

The canonical transition specification remains `pestoura/hermes-security-labs/docs/ghcr-private-readonly-transition.md` and tracking issue `#53`.
