### Question

You deploy an Azure Container Apps app and disable ingress on the container app.

Users report that they are unable to access the container app. You investigate and observe that the app has scaled to 0 instances.

You need to resolve the issue with the container app.

**Solution:** Enable ingress, create a custom scale rule, and apply the rule to the container app.

Does the solution meet the goal?

- [ ] Yes
- [ ] No

## Answer
- [ ] No

- because if ingress is disabled, and there is no custom rule, then the default scale rule comes into effect. And the minimum replica in the default rule is 0, which means the Container app will have no instances running

- The solution for this will be to create a custom rule, and set the minimum replica to 1 and apply the rule to the container app. Also, the custom rule works on the metrics from the container app. If the metric value crosses a certain threshold, then only it scales up. So if the minReplica is set to 0, the metric value will never cross the threshold. But setting the min replica to 1 is not part of the solution so it cannot help in resolving the issue

- [Resource](https://learn.microsoft.com/en-us/azure/container-apps/scale-app?pivots=azure-cli#default-scale-rule) 
---

## Detailed Explanation, with all terms

### ✅ What is Azure Container Apps?

Azure Container Apps is a service to run containerized applications without managing infrastructure. It automatically scales your app based on:

HTTP traffic (ingress)

Events from queues (like Azure Service Bus)

Or other custom KEDA-based triggers

📦 Example: You can deploy a Docker container to run a Node.js or .NET app, and it will scale up or down automatically.

### ✅ What does Ingress mean?

Ingress is the entry point for traffic from the internet (or other networks) to reach your container app.

In Azure Container Apps, ingress controls whether external users can send HTTP requests to the app.

- Ingress enabled → app has a public URL (like https://myapp.region.azurecontainerapps.io)

- Ingress disabled → no public access; app is internal or used only by other services inside Azure.

### ✅ What happens when you disable ingress?

When ingress is disabled:

- The app won't respond to external HTTP requests.

- Azure does not monitor for HTTP traffic, so it might scale to 0 (idle).

- You must use other triggers (like Service Bus, storage events, etc.) to keep it active.

### ✅ What does scaled to 0 instances mean?

Azure Container Apps uses serverless scaling. If there's no traffic or events:

- The app automatically scales down to 0 instances (no running containers).

- This is done to save resources and cost.

This is expected behavior unless you have a trigger to keep it alive.