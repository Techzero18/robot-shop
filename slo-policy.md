# Service Level Objective (SLO) & Error Budget Policy
**Service:** Robot Shop `web` & `catalogue` Tier  
**Owner:** DevOps Engineering  
**Review Cycle:** Rolling 30 Days  

## 1. Service Level Indicator (SLI)
The health of the Robot Shop frontend (`web`) and product (`catalogue`) tier is measured by **Availability (Success Rate)**:
* **Definition:** The proportion of valid HTTP requests that return a non-5xx HTTP status code (`status < 500`) and when the target service pods are ready and serving traffic (`up == 1`).
* **Formula:** `(Total Requests - 5xx Error Requests) / Total Requests * 100`

## 2. Service Level Objective (SLO)
* **Target:** **99.0% Availability** over a rolling 30-day window.

## 3. Error Budget Calculation
* **Allowed Failure Rate:** `100% - 99.0% = 1.0%`
* **30-Day Time Window:** `30 days × 24 hours × 60 minutes = 43,200 minutes`
* **Monthly Error Budget:** **432 minutes (7.2 hours)** of allowed downtime or degraded availability per 30-day window.

## 4. Error Budget Policy & Escalation Actions
1. **Green Zone (< 50% Budget Consumed / < 216 mins):**
   * Normal feature velocity and scheduled deployments proceed as planned.
2. **Yellow Zone (50% – 85% Budget Consumed / 216 – 367 mins):**
   * Automated Slack alert triggers to notify the on-call engineer.
   * All new deployments require mandatory peer review of Helm values and resource limits.
3. **Red Zone (> 85% or 100% Budget Exhausted / > 367 mins):**
   * **Feature Freeze:** All non-critical feature releases are halted immediately.
   * Engineering effort prioritizes reliability fixes, rollback verification, and completing the blameless postmortem before normal deployments resume.
