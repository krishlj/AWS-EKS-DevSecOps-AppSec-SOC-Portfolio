# 05 — Troubleshooting Guide

This document records the implementation problems captured during the project.

## 1. Docker Build — requirements.txt Not Found

### Symptom

```text
"/requirements.txt": not found
```

### Cause

The Docker build context did not contain the file expected by:

```dockerfile
COPY requirements.txt .
```

### Resolution

Ensure:

```text
Dockerfile
requirements.txt
app.py
```

are available in the build context.

Then rebuild:

```bash
docker build -t devsecops-app .
```

## 2. Flask Application

The local Flask application was successfully started and exposed on port `5000`.

![Application](../assets/local-app-running.png)

## 3. GitHub Actions Initial Failure

The GitHub Actions evidence shows an initial failed run followed by a successful run.

![Pipeline](../assets/github-actions-pipeline.png)

### Troubleshooting approach

1. Open the failed workflow.
2. Identify the first failing step.
3. Fix the repository/workflow configuration.
4. Commit the change.
5. Push to GitHub.
6. Re-run the workflow.
7. Verify all required steps.

## 4. EKS Cluster Provisioning

The EKS creation process was executed using `eksctl`.

![Cluster](../assets/eks-cluster-creation.png)

After provisioning, verify:

```bash
kubectl get nodes
kubectl get pods -A
```

## 5. Kubernetes Application Resources

The project evidence shows application pods, Service and other Kubernetes resources.

![Resources](../assets/kubernetes-resources.png)

Useful commands:

```bash
kubectl get pods
kubectl get svc
kubectl get endpoints
kubectl get ingress
kubectl describe ingress
```

## 6. Ingress / ALB Troubleshooting

The implementation required checking:

```text
Ingress
  ↓
Service
  ↓
Endpoints
  ↓
Pods
```

If the ALB exists but the application is unavailable, verify all four layers.

```bash
kubectl describe ingress <name>
kubectl get svc
kubectl get endpoints
kubectl get pods
```

## 7. Splunk PowerShell Parameter Error

An attempted PowerShell request used:

```text
-SkipCertificateCheck
```

The installed `Invoke-RestMethod` environment returned:

```text
A parameter cannot be found that matches parameter name 'SkipCertificateCheck'
```

### Lesson

PowerShell cmdlet parameters differ by PowerShell version/runtime.

Do not assume that an option available in one PowerShell version exists in another.

## 8. Splunk Protocol Violation

PowerShell returned:

```text
The server committed a protocol violation.
Section=ResponseStatusLine
```

This occurred during endpoint testing.

### Troubleshooting sequence

```text
Verify hostname
      ↓
Verify port
      ↓
Verify HTTP vs HTTPS
      ↓
Verify HEC path
      ↓
Verify token
      ↓
Test again
```

## 9. curl Test Errors

The implementation captured:

```text
Received HTTP/0.9 when not allowed
```

and an error indicating an invalid port format.

### Lesson

For HEC testing, use the complete HTTPS endpoint and quote the URL correctly.

## 10. Splunk Invalid Token

The application produced:

```text
Status Code: 403
Response: {"text":"Invalid token","code":4}
```

### Interpretation

The service was reachable, but the authentication credential was rejected.

Check:

- token value
- HEC input
- token permissions
- target index
- endpoint
- whether the token has been revoked

## 11. Successful Splunk Response

After correction:

```text
Status Code: 200
Response: {"text":"Success","code":0}
```

Events subsequently appeared in Splunk.

![Splunk Search](../assets/splunk-receives-app-logs.png)

## 12. Hardcoded Credential

One implementation screenshot visibly contained the HEC token.

For a public GitHub portfolio, this is not acceptable.

### Correct pattern

```text
.env                 -> local only
GitHub Secrets       -> CI/CD
Kubernetes Secret    -> runtime
AWS Secrets Manager  -> preferred production option
```

If the token was real, rotate it.

## 13. TLS Verification Warning

The application output included an `InsecureRequestWarning`.

### Lesson

Disabling TLS certificate verification may be useful during controlled troubleshooting, but should not be used as the production security configuration.

## 14. Development Server Warning

Flask displayed a warning that the development server should not be used in production.

### Production improvement

Use a WSGI server such as Gunicorn behind the Kubernetes service/ingress.

## 15. Trivy Vulnerability Count

The scan captured 104 vulnerabilities.

This does not mean the number will remain 104.

Vulnerability databases, packages and base images change over time.

The correct CI/CD control is to define a policy, for example:

```text
Fail build if CRITICAL vulnerabilities are detected.
```

The exact policy should match the organization's risk tolerance.
