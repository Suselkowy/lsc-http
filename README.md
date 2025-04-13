# Application Setup

This document outlines the steps required to set up the application.

## Prerequisites

- kubectl installed and configured to connect to your Kubernetes cluster.
- helm installed.

## Setup Instructions

1.  **Install NFS Provisioner with Storage Class `lsc`**

    This step deploys the NFS provisioner, which dynamically provisions Persistent Volumes.

    ```bash
    helm install nfs-provisioner nfs-ganesha-server-and-external-provisioner/nfs-server-provisioner --set=storageClass.name=lsc
    ```

2.  **Create Persistent Volume Claim**

    This command creates a PersistentVolumeClaim that will be used by the HTTP server deployment to request storage managed by the `lsc` StorageClass.

    ```bash
    kubectl apply -f .\PersistentVolumeClaim.yaml
    ```

3.  **Create HTTP Server Deployment**

    This deploys the HTTP server application.

    ```bash
    kubectl apply -f .\HttpServer.yaml
    ```

4.  **Create Service**

    This creates a Kubernetes Service to expose the HTTP server deployment.

    ```bash
    kubectl apply -f .\HttpService.yaml
    ```

5.  **Create Initializing Job**

    This job adds sample site to vlume.

    ```bash
    kubectl apply -f .\InitSiteJob.yaml
    ```

6.  **Get Site URL**

    To access the deployed application, retrieve the external IP or hostname of the service:

    ```bash
    kubectl -n lsc get services
    ```

    **Sample Output:**

    ```
    NAME                TYPE           CLUSTER-IP       EXTERNAL-IP                                                              PORT(S)        AGE
    nginx-nfs-service   LoadBalancer   10.100.121.247   a2850a6b0988148a3b7b5e8ed94f8944-763353058.us-east-1.elb.amazonaws.com   80:31320/TCP   4m58s
    ```

    Copy the value in the `EXTERNAL-IP` column.

7.  **Copy URL and Paste to Browser**

    Paste the copied URL into your web browser to access the application.
