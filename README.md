# AWS Cloud Engineer Portfolio

## AWS CloudTrail Security Audit

### How the pipeline works

**AWS API activity → CloudTrail → GitHub Actions → Grafana Loki → Grafana dashboard**

1. **AWS records activity.** CloudTrail captures AWS API calls, including role assumptions, CloudFormation stack operations, and Systems Manager parameter writes.

2. **GitHub Actions runs the audit.** A manually triggered workflow uses OIDC to assume a dedicated IAM role and queries CloudTrail Event History in `us-east-1` for the previous seven days.

3. **Python summarizes selected events.** The script counts `AssumeRoleWithWebIdentity`, `CreateStack`, `UpdateStack`, `DeleteStack`, and `PutParameter`, including zero counts.

4. **The workflow sends the summary to Loki.** Each successful run uploads one JSON summary over HTTPS using a stack-scoped, write-only Grafana token stored in GitHub Actions secrets.

5. **Grafana displays the results.** A Stat panel shows the latest summary’s total, and the audit run history table shows the per-API breakdown for each workflow run.

Counts reflect selected AWS API calls within a rolling seven-day window. GitHub pushes, pulls, syncs, and PR merges affect these counts when they lead to one of the monitored AWS API calls. CloudFormation API counts show requests to create, update, or delete a stack. Final deployment status requires a separate check.

### Dashboard

<img width="1542" height="509" alt="Grafana CloudTrail audit dashboard showing the rolling 7-day total and per-API counts" src="https://github.com/user-attachments/assets/efd8609c-4c8a-4aa0-9e2f-c9cfea2aeb99" />
