# Runbook: Robot Shop Service Availability Alert (`RobotShopServiceDown`)

## 1. Alert Overview
* **Alert Name:** `RobotShopServiceDown` / `SLO Availability Breach`
* **Severity:** Critical
* **Impact:** Users cannot browse products or complete checkouts on Stan's Robot Shop, actively burning the 99.0% monthly Error Budget.

## 2. Immediate Triage (First 2 Minutes)
1. **Check Pod Status in `robot-shop` Namespace:**
   ```bash
   sudo kubectl get pods -n robot-shop --kubeconfig /etc/rancher/k3s/k3s.yaml
