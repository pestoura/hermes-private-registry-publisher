# Security policy

## Scope

This repository contains private GHCR publication controls. Treat workflow changes, package permissions and credential handling as security-sensitive.

## Reporting

Report suspected credential exposure, unauthorized package access or workflow compromise privately to the repository owner. Do not place secrets, tokens, Docker authentication data, authorization headers or exploit details in issues, pull requests, commit messages or workflow logs.

## Required response

If secret exposure is suspected:

1. stop publication activity;
2. revoke or rotate the affected credential outside this repository;
3. preserve only sanitized evidence;
4. verify package visibility and repository access;
5. review Git history and workflow logs for exposure;
6. resume only after explicit owner approval.

## Prohibited content

Never commit:

- personal access tokens;
- GitHub CLI authentication material;
- Docker `config.json` authentication entries;
- private keys;
- passwords;
- authorization headers;
- decoded or encoded registry credentials.
