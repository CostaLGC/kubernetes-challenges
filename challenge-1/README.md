
# Kubernetes Challenge #4: Horizontal Pod Autoscaling (HPA)

In this challenge, you will set up **Horizontal Pod Autoscaling (HPA)** to automatically adjust 
the number of replicas in a Deployment based on CPU utilization. This exercise will help you understand 
how to set up and troubleshoot HPA, including using the Metrics Server.

---

## Challenge Requirements

### Step 1: Create a Deployment
1. **Create a Deployment** named `autoscale-deploy` with the following configurations:
   - Image: `nginx` (or another image of your choice).
   - CPU Limit: `100m`.
   - Optionally, add a CPU Request, e.g., `50m`, to make utilization calculations clearer.
   
2. **Create a Service** for the Deployment to expose it on port 80.

### Step 2: Configure Horizontal Pod Autoscaler
1. **Create an HPA** to automatically scale `autoscale-deploy` based on CPU utilization:
   - Minimum replicas: 1.
   - Maximum replicas: 5.
   - Target CPU utilization: 50%.
   
2. Use the following YAML structure for reference:
   ```yaml
   apiVersion: autoscaling/v2
   kind: HorizontalPodAutoscaler
   metadata:
     name: nginx-autoscaler
   spec:
     scaleTargetRef:
       apiVersion: apps/v1
       kind: Deployment
       name: autoscale-deploy
     minReplicas: 1
     maxReplicas: 5
     metrics:
     - type: Resource
       resource:
         name: cpu
         target:
           type: Utilization
           averageUtilization: 50
   ```

### Step 3: Install and Configure Metrics Server (if needed)
1. If not already installed, deploy the Metrics Server:
   ```bash
   kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
   ```

2. If you encounter TLS errors or see `<unknown>` as CPU target in HPA, modify the Metrics Server Deployment:
   - Add the following arguments to disable strict TLS verification and prioritize InternalIP:
     ```yaml
     - --kubelet-insecure-tls
     - --kubelet-preferred-address-types=InternalIP
     ```

### Step 4: Generate CPU Load to Test HPA
1. Use a temporary pod to create load:
   ```bash
   kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://nginx-service; done"
   ```

### Step 5: Monitor and Troubleshoot HPA
1. Use `kubectl get hpa nginx-autoscaler --watch` to observe replica adjustments.
2. If HPA shows `<unknown>` for CPU or does not scale:
   - Check Metrics Server status: `kubectl get pods -n kube-system | grep metrics-server`
   - Ensure CPU `requests` are configured in the Deployment.
   - Review HPA events for errors: `kubectl describe hpa nginx-autoscaler`.

---

**Expected Outcome:** The HPA should dynamically adjust replicas for `autoscale-deploy` based on CPU usage, with replicas increasing when utilization exceeds the target threshold.

**Note:** This challenge is designed to help you practice Kubernetes autoscaling and troubleshooting.
