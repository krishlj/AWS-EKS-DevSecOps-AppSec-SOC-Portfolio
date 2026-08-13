# 04 — SOC Workflow

## Objective

Turn application telemetry into actionable SOC detections.

## SOC Pipeline

```text
Application Request
       |
       v
Flask JSON Log
       |
       v
Splunk HEC
       |
       v
Index
       |
       v
SPL Detection
       |
       v
Alert
       |
       v
SOC Investigation
```

## Dashboard

![SOC Dashboard](../assets/splunk-soc-dashboard.png)

The dashboard contains:

- Total requests
- Top source IP addresses
- HTTP status codes
- Request trend
- Top endpoints

The supplied dashboard snapshot showed 833 total requests in its captured time window.

## Alert Configuration

![Alerts](../assets/splunk-alerts-overview.png)

The implementation contains four enabled alerts:

### 1. 404 Reconnaissance Detection

Purpose:

Identify repeated requests to endpoints that return HTTP 404.

Potential interpretation:

- endpoint discovery
- reconnaissance
- automated scanning
- broken client/application

### 2. Excessive Requests From Single IP

Purpose:

Identify high-volume request activity from one source.

Potential interpretation:

- brute force
- scanner
- abuse
- automation
- legitimate high-volume client

The alert requires contextual validation.

### 3. Kubernetes Events

Purpose:

Surface Kubernetes-related events for operational/security review.

Potential use cases:

- pod failures
- deployment changes
- scheduling problems
- unusual runtime events

### 4. Possible Brute Force Attack

Purpose:

Identify repeated failed authentication-related activity.

Potential investigation:

```text
Source IP
   |
Failed attempts
   |
Time window
   |
Target endpoint
   |
User/account context
   |
Correlated events
```

## 404 Investigation Example

The evidence shows events such as:

```text
/test   -> 404
/admin  -> 404
/login  -> 404
```

![404 Evidence](../assets/splunk-404-recon-evidence.png)

The browser-side `404 Not Found` response and Splunk-side event provide a useful example of end-to-end detection visibility.

## SOC Triage Procedure

### Step 1 — Validate

Confirm that the event exists in Splunk.

### Step 2 — Identify Source

Review:

```spl
index="main"
| stats count by ip
```

### Step 3 — Identify Target

Review:

```spl
index="main"
| stats count by endpoint
```

### Step 4 — Review Status

```spl
index="main"
| stats count by status_code
```

### Step 5 — Review Timeline

Look for bursts of requests.

### Step 6 — Correlate

Check:

- same IP
- same endpoint
- same user agent
- repeated failures
- Kubernetes events
- application errors

### Step 7 — Classify

Use:

```text
Benign
Suspicious
Confirmed malicious
False positive
```

### Step 8 — Respond

Depending on the environment:

- block/rate-limit source
- investigate application account
- inspect pod logs
- review ingress activity
- preserve evidence
- escalate incident

## Analyst Outcome

The purpose of the project is not simply to create charts. The workflow demonstrates:

```text
Telemetry
   ↓
Detection
   ↓
Alert
   ↓
Triage
   ↓
Decision
   ↓
Response
```
