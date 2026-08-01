# Private publisher boundary

## Purpose

This repository is the private publication and package-permission boundary for approved Hermes GHCR packages.

The public repository `pestoura/hermes-security-labs` remains canonical for source review, Compose definitions, manifests, accepted digests and runtime policy. It must not inherit or receive access to private packages.

## Current pilot

- Proposed package: `ghcr.io/pestoura/hermes-private-vampi`.
- Public rollback reference: `ghcr.io/pestoura/hermes-vampi@sha256:e7b2760d586ed2b4b15a689823a07816e32308bca293f9e8c08830c7b36c7229`.
- Canonical source baseline at repository creation: `pestoura/hermes-security-labs@b675c4c4f0f6a0c9ec22953a9cda8b5140b32db7`.

## Mandatory publication controls

Any future publication workflow must:

- run only on a GitHub-hosted runner;
- publish with this repository's `GITHUB_TOKEN`;
- request only the permissions required by the reviewed workflow;
- fetch immutable source and dependency revisions;
- use immutable tags and never publish `latest`, `main`, `master` or `develop`;
- generate and retain SBOM and provenance;
- link package permissions only to this private repository;
- perform no automatic deployment to Hermes;
- use no personal access token when `GITHUB_TOKEN` is sufficient;
- use no self-hosted runner or Hermes Docker socket.

## Separate authorization gates

Repository creation does not authorize:

1. adding or merging a publication workflow;
2. dispatching a publication workflow;
3. creating or changing a GHCR package;
4. creating a Hermes consumer credential;
5. installing registry credentials on Hermes;
6. changing Compose, manifests or runtime;
7. deleting, retagging or changing any accepted public package.

Each gate requires separate review and explicit owner authorization.

## Consumer boundary

Hermes consumption must use a distinct personal access token classic limited to `read:packages`. The current GitHub CLI OAuth token and any publication credential are forbidden for runtime pulls.

Credential values, Docker authentication data and authorization headers must never be committed, added to issues, retained in evidence or printed by workflows.
