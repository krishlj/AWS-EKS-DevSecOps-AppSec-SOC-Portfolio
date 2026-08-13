# 06 — Security Controls

## 1. Shift-Left Container Security

Trivy scans the container image before deployment.

![Trivy](../assets/trivy-image-scan.png)

## 2. CI/CD Security Gate

The CI/CD pipeline provides an automated place to enforce security checks.

Recommended policy:

```text
Build
  |
Trivy Scan
  |
Critical? ---- Yes ---> Fail pipeline
  |
 No
  |
Push to ECR
```

## 3. Secrets

The initial lab implementation demonstrated why credentials should not be embedded in source code.

### Do not commit

```text
.env
*.pem
*.key
kubeconfig
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
SPLUNK_HEC_TOKEN
```

### Preferred pattern

```text
GitHub Secrets
       |
       v
CI/CD
       |
       v
Kubernetes Secret / AWS Secrets Manager
       |
       v
Application
```

## 4. Kubernetes

Recommended controls:

- dedicated namespace
- ServiceAccount
- RBAC
- NetworkPolicy
- resource limits
- readiness/liveness probes
- non-root container
- read-only filesystem where possible
- image pinning
- vulnerability scanning

## 5. IAM

The architecture includes least-privilege IAM/IRSA concepts.

Avoid using broad administrator permissions for application workloads.

## 6. Logging

The application generates structured events that can be centrally searched.

This improves:

- visibility
- investigation
- detection
- auditability

## 7. Detection Engineering

The project implements detections for:

- 404 reconnaissance
- excessive requests
- Kubernetes events
- possible brute force

## 8. Security Improvements

Recommended production hardening:

1. Use Gunicorn instead of Flask development server.
2. Use HTTPS end-to-end.
3. Enable TLS certificate verification.
4. Store secrets outside source code.
5. Use immutable image tags/digests.
6. Enforce Trivy severity thresholds.
7. Add SAST and DAST.
8. Add runtime security monitoring.
9. Add AWS CloudTrail and GuardDuty.
10. Add incident response integrations.
