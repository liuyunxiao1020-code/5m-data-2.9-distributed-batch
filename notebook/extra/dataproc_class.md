## Google CLoud - Managed Apache Spark Cluster Setup

### Overview :

This runbook provisions a compact Dataproc Spark cluster that fits within the Free Tier footprint while still offering three e2-standard-2 nodes (one master and two workers) with 40 GB boot disks, component gateway, and the Jupyter/Zeppelin UIs enabled. The goal is to spin up an easily repeatable environment for PySpark experimentation against the 5M record batch dataset, favoring the us-east1 region for better capacity, and granting the minimum IAM required for both the operator account and the default Compute Engine service account.

### Create the cluster:
Copy and paste the following in cloud shell:
```
# --- 1. SET VARIABLES ---
export PROJECT_ID=$(gcloud config get-value project)
export PROJECT_NUMBER=$(gcloud projects describe $PROJECT_ID --format="value(projectNumber)")
export USER_EMAIL=$(gcloud config get-value account)
export CLUSTER_NAME="spark-cluster-1m-2w"

# CHANGING REGION: us-east1 often has more availability than us-central1
export REGION="us-east1"

# --- 2. ENABLE APIs & GRANT PERMISSIONS ---
gcloud services enable \
    dataproc.googleapis.com \
    compute.googleapis.com \
    storage-component.googleapis.com \
    storage.googleapis.com \
    iap.googleapis.com \
    cloudresourcemanager.googleapis.com

# User Permissions
gcloud projects add-iam-policy-binding $PROJECT_ID --member="user:$USER_EMAIL" --role="roles/dataproc.editor"
gcloud projects add-iam-policy-binding $PROJECT_ID --member="user:$USER_EMAIL" --role="roles/storage.objectAdmin"
gcloud projects add-iam-policy-binding $PROJECT_ID --member="user:$USER_EMAIL" --role="roles/iap.tunnelResourceAccessor"

# Service Account Permissions
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --member="serviceAccount:$PROJECT_NUMBER-compute@developer.gserviceaccount.com" \
    --role="roles/dataproc.worker"

# --- 3. CREATE THE CLUSTER ---
# CHANGING MACHINE TYPE: e2-standard-2 is more widely available than n1-standard-2

gcloud dataproc clusters create $CLUSTER_NAME \
    --region=$REGION \
    --master-machine-type=e2-standard-2 \
    --master-boot-disk-size=40GB \
    --num-workers=2 \
    --worker-machine-type=e2-standard-2 \
    --worker-boot-disk-size=40GB \
    --image-version=2.1-ubuntu20 \
    --enable-component-gateway \
    --optional-components=JUPYTER,ZEPPELIN
```
Wait approximately 2–4 minutes for the cluster to reach the RUNNING state.

## Run Jupyter notebook
 
Once the cluster is healthy, the next phases focus on opening the managed notebook interfaces for PySpark development, running the workload against GCS-hosted data, and cleaning up the cluster to avoid lingering charges. The guidance below walks through launching JupyterLab via the Dataproc component gateway, uploading the analysis notebook, and verifying the PySpark kernel before finally deleting the cluster when experimentation is complete.

```
1. Navigate to Dataproc > Clusters > $CLUSTER_NAME.
2. Click the Web Interfaces tab.
3. Click JupyterLab to open the interface.
4. Upload the prepared notebook : https://drive.google.com/file/d/1uu1Au1O7veqOPrZBpVZBKin9g28Pkq21/view?usp=share_link (download to local machine first, then upload to JupyterLab)
5. Click on the uploaded notebook in JupyterLab to open it.
6. Select the PySpark kernel to run Spark code from the notebook UI.
```

## Clean Up

To avoid unnecessary billing charges, delete the cluster when finished.

Run the following command in google cloud shell:
```Bash
gcloud dataproc clusters delete $CLUSTER_NAME --region=$REGION --quiet  
```


