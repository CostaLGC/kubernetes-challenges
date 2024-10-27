## Part 2: Responses and Solutions

### 1. Deployment, service and HPA Configuration

- **Deployment `deployment.yaml`**:
  ```yaml
  kubectl apply -f deployment.yaml
  ```

- **Service `service.yaml`**:
  ```yaml
  kubectl apply -f service.yaml
  ```

- **Horizontal Pod Autoscaler (HPA) `ascale-deploy`**:
  ```yaml
  kubectl apply -f auto-scale.yaml
  ```


### 2. Load Testing for Scaling Up

- We ran a temporary pod to generate CPU load on the `ascale-deploy` Deployment:
  ```bash
  kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://nginx-service; done"
  ```

- We observed the HPA scaling up replicas in response to the load using:
  ```bash
  kubectl get hpa nginx-autoscaler --watch
  ```

### 3. Scale Down Stabilization Adjustment

- When the load was removed, we noticed that the HPA took some time to scale down. To resolve this, we adjusted the HPA stabilization configuration:

  ```yaml
  spec:
    behavior:
      scaleDown:
        stabilizationWindowSeconds: 60
        policies:
        - type: Percent
          value: 100
          periodSeconds: 60
  ```

- After this adjustment, the HPA responded faster to the drop in load, reducing the number of replicas as expected.

### 4. Observed Results

- **Scaling Up**: The HPA increased the number of replicas when the load exceeded 50% CPU utilization.
- **Scaling Down**: After the stabilization adjustment, the HPA quickly decreased the replicas when the load was removed.

---

This document outlines the complete process, from initial setup to testing and fine-tuning the HPA. This exercise is an excellent example of how to configure and optimize HPA in Kubernetes using the Metrics Server.
