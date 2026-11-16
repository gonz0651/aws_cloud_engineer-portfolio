# AWS Cloud Engineer Portfolio

## AWS CloudTrail Security Audit

**AWS CloudTrail → GitHub Actions → Grafana Loki**

GitHub Actions uses OIDC to assume a dedicated AWS IAM role and query CloudTrail Event History in us-east-1. The workflow sends aggregate event counts to Grafana Loki using a stack-scoped, write-only token stored in GitHub Actions secrets.

The Stat panel shows the latest rolling 7-day total for five monitored API calls. The audit run history table shows the count for each API call by workflow run.
<img width="1542" height="509" alt="Grafana CloudTrail audit dashboard showing the rolling 7-day total and per-API counts" src="https://github.com/user-attachments/assets/efd8609c-4c8a-4aa0-9e2f-c9cfea2aeb99" />
