# Security Policy

This repository is an educational/portfolio implementation.

## Reporting

Do not publish credentials, private keys, AWS access keys, HEC tokens, kubeconfig files or other sensitive material.

## Before Publishing

- Rotate any credential that appeared in development screenshots.
- Scan the repository and Git history for secrets.
- Review `.env` and local configuration files.
- Remove account-specific infrastructure identifiers where unnecessary.

## Scope

Only test the application and infrastructure you own or are explicitly authorized to assess.
