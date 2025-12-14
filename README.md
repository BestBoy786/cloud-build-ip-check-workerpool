# ☁️ Cloud Build Private Worker Pool IP Checker

This repository contains a simple Google Cloud Build configuration (`cloudbuild.yaml`) designed to identify and log the **external egress IP address** of a custom **Private Worker Pool**.

This configuration is useful for:
* Verifying Network Address Translation (NAT) configurations for private pools.
* Debugging firewall rules or network policies that depend on the source IP of the build process.

## 🛠 Prerequisites

1.  **Google Cloud Project:** A working Google Cloud Project with the Cloud Build API enabled.
2.  **Private Worker Pool:** A pre-existing Cloud Build Private Worker Pool named `just-pool` in the `us-central1` region.
    * The configuration expects the pool to be named: `projects/$PROJECT_ID/locations/us-central1/workerPools/just-pool`.
3.  **Permissions:** The service account running the build must have the necessary permissions to access and use the specified worker pool.

## ⚙️ Configuration File (`cloudbuild.yaml`)

The build uses a single step to perform the IP check.

### Key Configuration Snippet:

```yaml
steps:
- name: 'gcr.io/cloud-builders/curl'
  id: 'get-worker-ip-formatted'
  entrypoint: 'sh'
  args:
    - '-s'
    - '-c'
    - 'printf "worker Pool IP : $(curl -s [https://curlmyip.org](https://curlmyip.org))\n"'

options:
  pool:
    name: 'projects/$PROJECT_ID/locations/$_LOCATION/workerPools/$_POOL_NAME'

substitutions:
  _LOCATION: 'us-central1'
  _POOL_NAME: 'just-pool'
