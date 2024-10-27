
# Challenge #5: Security Configurations and Data Persistence in Kubernetes

## Objective

In this challenge, you will configure a Deployment following best security practices, including sensitive environment variables, use of Secrets, and data persistence with volumes. The goal is to create a secure application with persistent data storage and control network permissions.

---

## Challenge Requirements

1. **Configure a Secure Pod**
   - Create a Deployment named `secure-app-deploy` with:
     - Image: `nginx` or an application of your choice.
     - At least one sensitive environment variable configured from a Secret.
     - An `emptyDir` volume mounted in the container for temporary storage.

2. **Configure a Persistent Volume**
   - Create a **PersistentVolume (PV)** and a **PersistentVolumeClaim (PVC)**:
     - The PV should have at least 1Gi of capacity and be of type `ReadWriteOnce`.
     - The PVC should be linked to the `secure-app-deploy` Deployment, mounted at `/data`.
   
3. **Network and Security Configuration**
   - Add a security policy to the Pod:
     - Define `securityContext` in the container to run as a specific user, e.g., UID `1001`.
     - Set `readOnlyRootFilesystem` to `true` to ensure the root filesystem is read-only.
   - Configure a **NetworkPolicy** to limit access:
     - Only allow connections to the Pod from pods in the same namespace with the same label `app: secure-app`.

4. **Security and Persistence Tests**
   - Validate the configuration:
     - Check that the `/data` volume is mounted correctly in the Pod and that data persists between Pod recreations.
     - Test access restrictions to the Pod using the NetworkPolicy.
   
---

## Response Structure

The response structure should include:
1. YAML manifests for Deployment, PersistentVolume, PersistentVolumeClaim, and NetworkPolicy.
2. Test commands to validate security configurations and data persistence.
3. Explanation of each step and how it meets the security and persistence requirements.
