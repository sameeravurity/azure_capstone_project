
# Task 3: Configure a Release Pipeline with Approvals and Gates

## Step 1: Configure a Release Pipeline

- Go to **Azure DevOps** and create a **Release Pipeline**.
- Add the required **artifacts** from the build pipeline.
- Define the **stages** where deployment will happen.

---

## Step 2: Configure Pre-Deployment Approvals

- Go to the **Pre-deployment conditions** of a stage.
- Enable **Pre-deployment approvals**.
- Add yourself as the **approver**.

---

## Step 3: Configure Post-Deployment Gates

- Under the same stage, go to **Post-deployment conditions**.
- Enable **Gates**.
- Add a gate condition to check for **Azure Monitor Alerts**.
- Link the gate to a **metric-based alert rule**.

---

## Step 4: Create and Enable an Alert Rule

1. Go to the **Azure Portal**.
2. Navigate to **Monitor > Alerts > Alert Rules**.
3. Create a new alert rule based on a metric (e.g., CPU usage or availability).
4. Enable the alert rule so that it can be triggered during deployment.

---

## Step 5: Generate Active Alerts

- Simulate the condition that triggers the alert (e.g., stress the resource).
- Wait for the alert to become **active**.

🛑 **Expected Outcome**:  
The **post-deployment gate** should **fail** because the alert is active.

---

## Step 6: Close the Alert

- Go to the alert in the Azure Portal.
- Change the **user response** of the alert to **closed**.

✅ **Expected Outcome**:  
The **post-deployment gate** should now **pass**, allowing the release to proceed.

---

This setup ensures that deployments are halted when critical alerts are active, enforcing better release quality and monitoring.
