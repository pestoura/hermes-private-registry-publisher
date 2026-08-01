# Hermes Private Registry Publisher

Private GitHub repository for reviewed publication workflows that create private GHCR packages consumed by Hermes.

## Security boundary

- Public canonical source: `pestoura/hermes-security-labs`.
- Private publisher and package-permission boundary: this repository.
- First proposed package: `ghcr.io/pestoura/hermes-private-vampi`.
- Publication must use this repository's ephemeral `GITHUB_TOKEN`.
- Hermes runtime access must use a separate credential limited to `read:packages`.
- No self-hosted runner, Hermes Docker socket or automatic deployment is permitted.

## Current status

Repository boundary created. No publication workflow, package, token, secret or deployment has been created.

The canonical transition specification is maintained in `pestoura/hermes-security-labs` under issue `#53` and `docs/ghcr-private-readonly-transition.md`.
