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

## Current visibility drift

As of 2026-08-09 this repository is temporarily `public` for audit access. **That is not the accepted target state.**

Before any Hermes `read:packages` credential is provisioned, authenticated private pull is attempted, or Compose migration begins, this repository must be restored to `private` and that state must be re-verified.

The package itself must remain private throughout. Making the package public would permanently reject that package identity for the private target state.

## Remaining gate

The linked GitHub integration cannot read the package repository-access list and receives `403 Resource not accessible by integration` from the Packages API. Therefore issue `pestoura/hermes-security-labs#53` remains blocked before credential provisioning until both conditions are recorded:

1. this publisher repository is restored to `private`;
2. GitHub Package Settings confirm that no unapproved repository, user or team has access and that `pestoura/hermes-security-labs` has no package access.

After those conditions pass, the controlled sequence is: authenticated exact-digest read-only pull → safe negative control for write/delete authority → temporary lifecycle parity → reviewed Compose migration → exact-SHA post-merge acceptance → rollback proof → deployment tracking reconciliation.

The canonical transition specification remains `pestoura/hermes-security-labs/docs/ghcr-private-readonly-transition.md` and tracking issue `#53`.
