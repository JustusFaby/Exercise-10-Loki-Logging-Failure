# Exercise 10 – Loki Logging Failure Troubleshooting

## Objective

Troubleshoot a logging pipeline failure where application logs stopped appearing in Grafana.

---

## Incident

### Alloy Logs

```text
failed to push logs
HTTP 403
```

### Loki Logs

```text
authentication failed
```

---

## Logging Flow

```text
Application
    ↓
Alloy
    ↓
Loki
    ↓
Grafana
```

---

## Problem Analysis

The logging pipeline consists of:

* **Application** – Generates logs
* **Alloy** – Collects and forwards logs
* **Loki** – Stores logs
* **Grafana** – Visualizes logs

The incident indicates that Alloy was unable to successfully send logs to Loki due to an authentication issue.

---

## Failure Point

```text
Application ✓
    ↓
Alloy ✗
    ↓
Loki
    ↓
Grafana
```

The failure occurs between **Alloy** and **Loki**.

---

## Root Cause

Incorrect authentication configuration between Alloy and Loki caused requests from Alloy to be rejected, preventing logs from reaching Loki and Grafana.

---

## Troubleshooting Steps Performed

### 1. Verified Loki Deployment

```bash
kubectl get pods -n logging
```

### 2. Verified Alloy Deployment

```bash
kubectl describe deployment alloy -n logging
```

### 3. Verified ConfigMap Mount

Confirmed Alloy was loading configuration from:

```text
/etc/alloy/config.alloy
```

### 4. Verified Alloy Configuration

```bash
kubectl exec -it deployment/alloy -n logging -- cat /etc/alloy/config.alloy
```

### 5. Checked Alloy Logs

```bash
kubectl logs deployment/alloy -n logging
```

---

## Resolution

1. Verified Alloy configuration.
2. Confirmed ConfigMap was mounted correctly.
3. Validated Loki endpoint configuration.
4. Identified authentication configuration as the root cause.
5. Updated configuration and restarted Alloy when required.

---

## Commands Used

```bash
kubectl get pods -n logging

kubectl describe deployment alloy -n logging

kubectl exec -it deployment/alloy -n logging -- cat /etc/alloy/config.alloy

kubectl logs deployment/alloy -n logging
```

---

## Expected Outcome

* Alloy successfully forwards logs to Loki.
* Loki stores incoming logs.
* Grafana displays logs without interruption.

---

## Demonstration Video

**Demo Link:**
https://drive.google.com/file/d/1r_bFNsdVJhICBUXY9-fQ9gQn_c7QAJyX/view?usp=sharing

---

## Author

**Justus Faby Jeyakumar**
GitHub: https://github.com/JustusFaby
