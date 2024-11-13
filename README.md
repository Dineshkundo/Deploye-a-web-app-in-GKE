# Bank of Anthos

![GitHub branch check runs](https://img.shields.io/github/check-runs/GoogleCloudPlatform/bank-of-anthos/main)
[![Website](https://img.shields.io/website?url=https%3A%2F%2Fcymbal-bank.fsi.cymbal.dev%2F&label=live%20demo
)](https://cymbal-bank.fsi.cymbal.dev)

**Bank of Anthos** is a sample HTTP-based web app that simulates a bank's payment processing network, allowing users to create artificial bank accounts and complete transactions.

Google uses this application to demonstrate how developers can modernize enterprise applications using Google Cloud products, including: [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine),
This application works on any Kubernetes cluster.

## Screenshots

| Sign in                                                                                                        | Home                                                                                                    |
| ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| ![image](https://github.com/user-attachments/assets/15c75e7c-7f39-4142-8c6f-6ee8dfe80c7d)| ![image](https://github.com/user-attachments/assets/904c4776-fcf6-44bd-88f7-48629b41f702)|


![image](https://github.com/user-attachments/assets/5b8c8e95-028c-4d25-a1e9-de1f3230447e)

![image](https://github.com/user-attachments/assets/bfb89e0a-1679-41bf-b0fd-ff35bf869ee8)

## Interactive quickstart (GKE)

The following button opens up an interactive tutorial showing how to deploy Bank of Anthos in GKE:

[![Open in Cloud Shell](https://gstatic.com/cloudssh/images/open-btn.svg)](https://ssh.cloud.google.com/cloudshell/editor?show=ide&cloudshell_git_repo=https://github.com/GoogleCloudPlatform/bank-of-anthos&cloudshell_workspace=.&cloudshell_tutorial=extras/cloudshell/tutorial.md)

## Quickstart (GKE)

1. Ensure you have the following requirements:
   - [Google Cloud project](https://cloud.google.com/resource-manager/docs/creating-managing-projects#creating_a_project).
   - Shell environment with `gcloud`, `git`, and `kubectl`.

2. Clone the repository.

   ```sh
   git clone https://github.com/GoogleCloudPlatform/bank-of-anthos
   cd bank-of-anthos/
   ```

3. Set the Google Cloud project and region and ensure the Google Kubernetes Engine API is enabled.

   ```sh
   export PROJECT_ID=<PROJECT_ID>
   export REGION=us-central1
   gcloud services enable container.googleapis.com \
     --project=${PROJECT_ID}
   ```

   Substitute `<PROJECT_ID>` with the ID of your Google Cloud project.

4. Create a GKE cluster and get the credentials for it.

   ```sh
   gcloud container clusters create-auto bank-of-anthos \
     --project=${PROJECT_ID} --region=${REGION}
   ```

   Creating the cluster may take a few minutes.

5. Deploy Bank of Anthos to the cluster.

   ```sh
   kubectl apply -f ./extras/jwt/jwt-secret.yaml
   kubectl apply -f ./kubernetes-manifests
   ```

6. Wait for the pods to be ready.

   ```sh
   kubectl get pods
   ```

   After a few minutes, you should see the Pods in a `Running` state:

   ```
   NAME                                  READY   STATUS    RESTARTS   AGE
   accounts-db-6f589464bc-6r7b7          1/1     Running   0          99s
   balancereader-797bf6d7c5-8xvp6        1/1     Running   0          99s
   contacts-769c4fb556-25pg2             1/1     Running   0          98s
   frontend-7c96b54f6b-zkdbz             1/1     Running   0          98s
   ledger-db-5b78474d4f-p6xcb            1/1     Running   0          98s
   ledgerwriter-84bf44b95d-65mqf         1/1     Running   0          97s
   loadgenerator-559667b6ff-4zsvb        1/1     Running   0          97s
   transactionhistory-5569754896-z94cn   1/1     Running   0          97s
   userservice-78dc876bff-pdhtl          1/1     Running   0          96s
   ```

7. Access the web frontend in a browser using the frontend's external IP.

   ```sh
   kubectl get service frontend | awk '{print $4}'
   ```

   Visit `http://EXTERNAL_IP` in a web browser to access your instance of Bank of Anthos.

8. Once you are done with it, delete the GKE cluster.

   ```sh
   gcloud container clusters delete bank-of-anthos \
     --project=${PROJECT_ID} --region=${REGION}
   ```

   Deleting the cluster may take a few minutes.
