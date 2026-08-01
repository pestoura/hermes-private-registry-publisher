# Private VAmPI publication pilot

## Purpose

Publish `ghcr.io/pestoura/hermes-private-vampi` through this permanently private repository without changing the active Hermes runtime.

This pilot is a finite supply-chain control supporting the public canonical project. It must not block Phase 2 laboratory implementation.

## Immutable inputs

| Input | Value |
|---|---|
| Canonical repository | `pestoura/hermes-security-labs` |
| Canonical source commit | `b675c4c4f0f6a0c9ec22953a9cda8b5140b32db7` |
| Upstream repository | `https://github.com/erev0s/VAmPI` |
| Upstream commit | `f16052dce83f05847133ec98f01c5193a41de7d8` |
| Upstream Dockerfile SHA-256 | `555b6d28d7e3c4bd39c75d6a9d102aca9a0c08a8011ae7f780c6b58b167e827a` |
| Public rollback digest | `ghcr.io/pestoura/hermes-vampi@sha256:e7b2760d586ed2b4b15a689823a07816e32308bca293f9e8c08830c7b36c7229` |

## Workflow boundary

The workflow is `.github/workflows/publish-private-vampi.yml`.

It:

- is available only through manual `workflow_dispatch`;
- requires the operator to set `publish=true`;
- runs only on a GitHub-hosted runner;
- verifies the immutable upstream Dockerfile hash before registry login;
- publishes with this repository's ephemeral `GITHUB_TOKEN`;
- requests only `contents: read` and `packages: write`;
- publishes immutable upstream and publisher-commit tags;
- generates BuildKit provenance and SBOM;
- records publisher, canonical-source and upstream metadata separately;
- does not access or deploy to Hermes.

## First-publication gate

Merging the workflow does not itself authorize or execute publication.

Before the first dispatch:

1. confirm this repository remains private;
2. confirm GitHub-hosted Actions are available and no account budget block applies;
3. confirm `hermes-private-vampi` does not already exist under the target namespace;
4. confirm the public rollback digest remains available;
5. review the exact workflow commit to be dispatched;
6. dispatch once with `publish=true`.

## Package acceptance

Immediately after publication, verify:

- visibility is exactly `private`;
- package linkage and inherited access reference only `pestoura/hermes-private-registry-publisher`;
- `pestoura/hermes-security-labs` has no package Actions, read, write or admin access;
- no unapproved repository, user or team has access;
- OCI index, application manifest and attestation manifests are identified separately;
- the OCI index digest matches the workflow summary;
- `linux/amd64` is present;
- provenance and SBOM attestations are present;
- all required OCI and `io.hermes.*` labels are present.

Any public visibility result permanently rejects this package identity for the private target state.

## Access validation

Before any Hermes credential is created:

1. prove anonymous metadata inspection or pull fails;
2. record only the sanitized denial result;
3. do not change package visibility as a troubleshooting step.

Hermes credential provisioning is a separate host-operation gate. The credential must be a PAT classic limited to `read:packages`, stored outside Git and supplied to Docker through standard input and an isolated Docker configuration.

## Runtime boundary

This issue does not modify Compose, manifests or the running VAmPI laboratory.

After private package acceptance, the public project must separately perform:

1. authenticated exact-digest pull on Hermes;
2. image and OCI metadata inspection;
3. lifecycle parity through a temporary ignored override;
4. isolated Kali connectivity validation;
5. final destroyed-state and unrelated-resource drift checks;
6. a reviewed Compose migration PR;
7. post-merge acceptance;
8. rollback demonstration to the accepted public digest.

## Exit

The private VAmPI pilot is complete when publication, package privacy, private linkage, anonymous denial and immutable digest evidence pass without credential exposure.

The remaining private image migrations proceed one at a time and do not block Phase 2 laboratory development in `pestoura/hermes-security-labs#9`.
