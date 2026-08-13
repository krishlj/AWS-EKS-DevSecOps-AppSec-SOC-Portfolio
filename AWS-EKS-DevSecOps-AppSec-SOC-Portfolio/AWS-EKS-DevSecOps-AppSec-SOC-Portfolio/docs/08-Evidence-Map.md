# 08 — Evidence Map

This page maps the supplied screenshots and dashboard PDF to the implementation stages.

| Evidence file | Portfolio section | What it demonstrates |
|---|---|---|
| `architecture-diagram.png` | Architecture | End-to-end DevSecOps + SOC architecture |
| `local-app-running.png` | Application | Flask application running locally |
| `docker-build-troubleshooting.png` | Troubleshooting | Docker build issue |
| `eks-cluster-creation.png` | Deployment | EKS cluster creation |
| `eks-application-running.png` | Deployment | Application running through EKS/ALB |
| `kubernetes-resources.png` | Kubernetes | Pods/services/resources |
| `kubernetes-show-commands.png` | Kubernetes | CLI verification commands |
| `github-actions-pipeline.png` | CI/CD | GitHub Actions workflow evidence |
| `trivy-image-scan.png` | Security | Container vulnerability scan |
| `splunk-cloud-console.png` | Splunk | Splunk Cloud environment |
| `app-splunk-hec-config-redacted.png` | Splunk | HEC application configuration, credential redacted |
| `app-splunk-hec-debug.png` | Troubleshooting | HEC integration testing |
| `splunk-receives-app-logs.png` | Logging | Events successfully visible in Splunk |
| `splunk-search-results.png` | Logging | SPL search/event view |
| `splunk-404-recon-evidence.png` | Detection | 404 reconnaissance evidence |
| `splunk-soc-dashboard.png` | SOC | Security operations dashboard |
| `splunk-alerts-overview.png` | SOC | Four configured alerts |
| `splunk-alerts-evidence-2.png` | SOC | Additional alert evidence |
| `splunk-alerts-evidence-3.png` | SOC | Additional alert evidence |
| `devsecops-soc-dashboard.pdf` | SOC | PDF snapshot of dashboard |
| `architecture-diagram.png` | Architecture | Security and detection layers |

## Evidence Narrative

### Application

The screenshots demonstrate that the Flask application was successfully executed and accessed through a browser.

### Container

Docker evidence demonstrates image creation and execution, along with a real build-context troubleshooting case.

### Kubernetes

EKS and Kubernetes screenshots demonstrate cluster provisioning, application pods, services and ingress.

### CI/CD

The GitHub Actions screenshot demonstrates iterative pipeline execution, including a successful workflow.

### Security

The Trivy screenshot provides evidence of container vulnerability scanning.

### SIEM

Splunk screenshots demonstrate:

```text
Application
   ↓
HEC
   ↓
Splunk Index
   ↓
Search
   ↓
Dashboard
```

### Detection

The alert screenshots demonstrate that the project moved beyond log collection into basic detection engineering.

## Public Repository Safety

The original implementation evidence included a visible HEC token in one screenshot.

The portfolio copy uses:

```text
app-splunk-hec-config-redacted.png
```

The credential is redacted.

Before publishing, also inspect Git history for secrets.
