Incident Postmortem: Robot Shop Availability Breach (Drill)
Incident Date: October 25, 2026

Author: Joshua Fakunwade

Status: Resolved / Completed Successfully

Severity: Sev-2 (Simulated Critical / Drill)
1. Incident Summary
On October 25, 2026, a scheduled chaos engineering and incident response drill was executed on Stan's Robot Shop Kubernetes cluster (K3s on AWS EC2). The objective was to test the end-to-end observability stack (Prometheus, Loki, Grafana), verify live Slack alerting via Webhooks, and practice rapid mitigation procedures outlined in the team runbook.
2. Impact & Detection
User Impact: None (controlled staging/capstone environment).

SLO Impact: Simulated breach of the 99.0% availability target for core services (web / catalogue), triggering error budget consumption metrics.

Detection: The alert rule RobotShopAvailabilitySLOBreach detected the dropped readiness state and synthetic error log within 20 seconds, successfully routing a critical notification directly to the #new-channel Slack channel containing a direct link to the incident runbook.
3. Incident Timeline (All times local)
11:30:00 — Incident Injected: Synthetic error logs emitted into the robot-shop namespace and the catalogue deployment scaled to 0 replicas to simulate a database/service failure.

11:30:20 — Alert Fired: Grafana evaluated the threshold condition and triggered the webhook notification to Slack.

11:31:00 — Triage Initiated: On-call engineer reviewed the Slack alert summary and opened the linked runbook.md.

11:31:45 — Mitigation Executed: Service restored by scaling the catalogue deployment back to 1 replica.

11:32:30 — Resolution Confirmed: Pod readiness probes passed, and log error rates returned to baseline.
4. Root Cause Analysis
This was a deliberate, controlled failure injection rather than an organic software bug.

Immediate Cause: Intentional termination of the catalogue microservice pods and introduction of connection refusal error logs.

Contributing Factors: Designed to validate that our automated alerting pipeline functions correctly under failure conditions and that our runbook instructions are clear and actionable.

5. Lessons Learned & What Went Well
What Went Well:

The Prometheus and Loki metric queries caught the failure state almost instantly.

The Slack notification arrived promptly with the runbook link attached, minimizing time-to-triage.

The mitigation commands (kubectl scale) restored service rapidly.

6. Action ItemsIDAction ItemOwnerTarget DateStatusAI-01Add automated pod restart policies to prevent manual intervention for transient database drops.DevOps TeamCompletedDoneAI-02Ensure Grafana annotations and runbook links are permanently maintained in version control.Documentation LeadCompletedDone
7. 
Areas for Improvement:

Ensure all team members have immediate access to the GitHub runbook without friction during high-stress scenarios.
