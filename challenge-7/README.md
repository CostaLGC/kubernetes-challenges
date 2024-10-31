
# Kubernetes Volume Challenge #7

**Objective:**  
Create a Kubernetes environment where multiple pods share a persistent volume to simulate a logging application. Additionally, configure a Secret as a volume to provide authentication credentials used at pod startup.

## Requirements

1. **Persistent Volume:**
   - Create a PersistentVolume (PV) with 500Mi of storage and `ReadWriteMany` access mode so multiple pods can access it.
   - Create a PersistentVolumeClaim (PVC) linked to the PV for the pods to use this volume.

2. **Logging Pods:**
   - Configure three pods called `log-writer`, `log-reader`, and `log-backup`.
   - All pods should mount the persistent volume at the path `/var/logs`.

     - **log-writer:** This pod should write test logs to a file named `/var/logs/app.log` every 5 seconds.
     - **log-reader:** This pod should read the contents of `/var/logs/app.log` and output it to the pod logs.
     - **log-backup:** This pod should monitor the file `/var/logs/app.log` and copy its contents to a backup file at `/var/logs/backup.log` every 15 seconds.

3. **Secret for Credentials:**
   - Create a Secret named `auth-secret` with keys `username` and `password`.
   - Mount the Secret as a volume in `/etc/auth` in all pods to simulate reading credentials.

4. **Data Persistence:**
   - Ensure the file `/var/logs/app.log` retains its data even if `log-writer` and `log-reader` pods are restarted.

## Tasks

- Create YAML manifests for the PV, PVC, Secret, and pod configurations.
- Apply the manifests and test functionality:
  - Verify the Secret is correctly mounted in all pods.
  - Test writing, reading, and backing up operations on the persistent volume.
  - Check that `log-reader` logs display the content of `app.log`.
  - Restart `log-writer` and `log-reader` pods to confirm log persistence.

## Tips and Useful Commands

- To monitor logs from pods:
  ```bash
  kubectl logs -f <pod-name>
  ```

- To restart a specific pod:
  ```bash
  kubectl delete pod <pod-name>
  ```

This challenge will help reinforce knowledge about shared volumes, Secrets, and data persistence across multiple pods in Kubernetes. Good luck!
