# Hermes Private Registry Publisher

Publication boundary for reviewed GitHub Actions workflows that create private GHCR packages consumed by Hermes.

## Security boundary

- Public canonical source: `pestoura/hermes-security-labs`.
- Target publisher and package-permission boundary: this repository, **private in the accepted operating state**.
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

## Accepted private boundary

As of 2026-08-09 the publisher repository has been restored to `private` and that visibility has been re-verified through the GitHub repository API.

The owner has separately confirmed the GitHub Package Settings gate for `hermes-private-vampi`: the package remains private, the private publisher is the intended repository access boundary, the public canonical source repository has no package access, and no unapproved repository, team or user access is present.

The linked GitHub integration still receives `403 Resource not accessible by integration` from the Packages API. Therefore package repository-access details are recorded as **owner-confirmed**, not API-verified. This limitation does not override or weaken the independent runtime access controls.

The package itself must remain private throughout. Making the package public would permanently reject that package identity for the private target state.

## Remaining operational gate

Repository visibility and Package Settings preconditions are now satisfied at their respective evidence levels.

The next controlled sequence is:

1. provision a distinct Hermes consumer credential with exactly `read:packages`;
2. authenticate through stdin using an isolated Docker configuration;
3. prove exact-digest private pull;
4. prove absence of push/delete authority without package mutation;
5. run temporary private VAmPI lifecycle parity;
6. review and merge the separate Compose migration;
7. perform exact-SHA post-merge acceptance;
8. demonstrate rollback to the accepted public digest;
9. reconcile deployment tracking and drift detection.

No consumer PAT is stored in this repository or in GitHub Actions for the runtime acceptance path.

The canonical transition specification remains `pestoura/hermes-security-labs/docs/ghcr-private-readonly-transition.md` and tracking issue `#53`.
