# Google Cloud Commands and Troubleshooting Guide

This document contains useful Google Cloud commands and solutions to common issues encountered during deployment. A great read if you are just starting out with Google Cloud.

First Step is to [Install the Google Cloud CLI](https://cloud.google.com/sdk/docs/install-sdk)

---

## **Important Google Cloud Commands**

### **Authentication**
- **Login to Google Cloud:**
  ```bash
  gcloud auth login
  ```
  Authorize Google Cloud access for the `gcloud` CLI with your credentials.

- **Set the active project:**
  ```bash
  gcloud config set project [PROJECT_ID]
  ```
  Set the active project for your `gcloud` CLI commands.

---

### **App Engine**
- **Deploy an App Engine app:**
  ```bash
  gcloud app deploy
  ```
  Deploys your app using the `app.yaml` configuration file.

- **View App Engine logs:**
  ```bash
  gcloud app logs tail -s default
  ```
  Streams logs from your App Engine app.

- **Open deployed app in a browser:**
  ```bash
  gcloud app browse
  ```
  Opens your deployed app in your default browser.

---

### **Projects**
- **List all projects:**
  ```bash
  gcloud projects list
  ```
  Displays all projects linked to your account.

- **Create a new project:**
  ```bash
  gcloud projects create [PROJECT_ID]
  ```
  Creates a new project with the specified `[PROJECT_ID]`.

- **Delete a project:**
  ```bash
  gcloud projects delete [PROJECT_ID]
  ```
  Deletes the specified project.

---

### **Storage**
- **List storage buckets:**
  ```bash
  gcloud storage buckets list
  ```

- **Create a storage bucket:**
  ```bash
  gcloud storage buckets create [BUCKET_NAME]
  ```

- **Delete a storage bucket:**
  ```bash
  gcloud storage buckets delete [BUCKET_NAME]
  ```

---

### **Compute Engine (VM Instances)**
- **List instances:**
  ```bash
  gcloud compute instances list
  ```

- **Start an instance:**
  ```bash
  gcloud compute instances start [INSTANCE_NAME]
  ```

- **Stop an instance:**
  ```bash
  gcloud compute instances stop [INSTANCE_NAME]
  ```

---

### **Logs and Monitoring**
- **View logs for errors in App Engine:**
   ```bash
   gcloud logging read "resource.type=gae_app AND severity>=ERROR" --limit=10 --format=json
   ```

---

## **Troubleshooting**

### Issue: `gcloud.app.deploy` Error Response - Failed to Create Cloud Build (Invalid Bucket)

#### Error Message:
```
ERROR: (gcloud.app.deploy) Error Response: Failed to create cloud build: invalid bucket "1012918181174.cloudbuild-logs.googleusercontent.com"; builder service account does not have access to the bucket.
```

#### Cause:
This error occurs when the default service account used by App Engine or Cloud Build does not have the necessary permissions to access the required storage bucket for build logs.

#### Solution:
1. Go to the [Cloud Build Settings](https://console.cloud.google.com/cloud-build/settings) page for your project.
2. Enable the `App Engine Admin` service.
3. Grant appropriate permissions to the default service account:
   - Go to the [IAM & Admin](https://console.cloud.google.com/iam-admin) page.
   - Find the default App Engine service account (`[PROJECT_ID]@appspot.gserviceaccount.com`) and grant it the following roles:
     - `Storage Admin`
     - `Storage Object Admin`
4. Redeploy your app:
   ```bash
   gcloud app deploy
   ```

#### Alternate Fix:
If enabling permissions doesn't work, try repairing the App Engine setup:
```bash
gcloud beta app repair
```

This command repairs common issues with App Engine configurations, including missing permissions and invalid buckets.

---

## Notes:
1. Always ensure that all required APIs (e.g., App Engine, Cloud Build) are enabled in your project.
2. Check logs for detailed error information if deployment fails:
   ```bash
   gcloud app logs tail -s default
   ```
