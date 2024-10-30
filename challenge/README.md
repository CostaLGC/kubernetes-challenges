
# Kubernetes Volume Challenge #6

## Objective
Set up a Kubernetes environment where two pods share a persistent volume to simulate a file read-write application. Additionally, a ConfigMap will be mounted as a read-only volume to provide startup configurations for both pods.

## Requirements

1. **Create a PersistentVolume (PV) and PersistentVolumeClaim (PVC):**
   - Configure a **PV** with 1Gi storage and `accessMode` set to **ReadWriteMany** (allowing multiple pods to access the volume).
   - Create a **PVC** for the PV, so both pods can use this volume.

2. **Deploy the Pods:**
   - Create two pods, `writer-pod` and `reader-pod`, which will use the PVC.
   - Both pods should mount the persistent volume to `/data`.
   - `writer-pod`: This pod should write a message to a file called `/data/message.txt` every 10 seconds.
   - `reader-pod`: This pod should read the content of the file `/data/message.txt` and output it to the logs.

3. **ConfigMap for Configuration:**
   - Create a `ConfigMap` with a key called `app-config` that contains the following content:
     ```plaintext
     Message from ConfigMap: Welcome to the Volume Challenge!
     ```
   - Mount the `ConfigMap` as a volume on both pods at the `/config` directory.
   - Both pods should log the content of this file on startup.

4. **Data Persistence:**
   - To simulate persistence, ensure that `/data/message.txt` continues to exist even if the pods are restarted.
   - Confirm the volume persistence by verifying that `writer-pod`'s written content remains accessible to `reader-pod` after both pods are restarted.

---

## Tasks

1. **Create the YAML Manifests** for the PV, PVC, ConfigMap, and deployments for `writer-pod` and `reader-pod`.
2. **Apply and Test** the deployments and volumes:
   - Verify that the `ConfigMap` is mounted correctly in both pods.
   - Test reading and writing on the persistent volume.
3. **Check Logs**:
   - Capture logs from `reader-pod` to confirm it can read messages written by `writer-pod`.
   - Restart both pods and ensure the content of `/data/message.txt` persists.

---

### Tips and Useful Commands

- Monitor pod logs:
  ```bash
  kubectl logs -f <pod-name>
  ```
- To restart a specific pod:
  ```bash
  kubectl delete pod <pod-name>
  ```

This challenge will help you understand shared volumes and data persistence in a realistic scenario. Let me know if you need any guidance!
