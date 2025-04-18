# Kubernetes Application with NFS Provisioning

This repository contains a simple Kubernetes application with NFS provisioning. The application is composed of the following components:

- **NFS Server Provisioner**: Dynamically provisions Persistent Volumes (PV) using NFS.
- **Persistent Volume Claim (PVC)**: A claim that binds to the dynamically provisioned NFS volume.
- **HTTP Server Deployment (nginx)**: A basic HTTP server (nginx) serving web content.
- **Job**: A job that copies sample content into the NFS volume.
- **Service**: A Kubernetes service exposing the HTTP server to the outside world.

## Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/) (or another Kubernetes cluster)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
- [Helm](https://helm.sh/docs/intro/install/)

## Installation

1. **Start Kubernetes Cluster**:
    - If you are using Minikube:
      ```bash
      minikube start --driver=docker
      ```
    - For other Kubernetes clusters, make sure kubectl is configured to point to your cluster.

2. **Install Helm Chart for NFS Server Provisioner**:
   Add the NFS Server Provisioner to your cluster using Helm. This chart provisions an NFS server dynamically.
   ```bash
   helm install nfs-server-provisioner stable/nfs-server-provisioner --set nfs.server=true --set persistence.enabled=true

3. **Create Persistent Volume Claim (PVC)**:
   Apply the PVC configuration to bind the persistent volume provisioned by the NFS server.
   ```bash
   kubectl apply -f kubernetes/pvc.yaml

4. **Deploy HTTP Server (nginx)**:
   Apply the Deployment configuration to deploy an nginx HTTP server.
   ```bash
   kubectl apply -f kubernetes/deployment.yaml

5. **Create Service**:
   Apply the Service configuration to expose the HTTP server.
   ```bash
   kubectl apply -f kubernetes/service.yaml

6. **Create Job to Copy Sample Content**:
   Apply the Job configuration to copy sample content to the NFS volume.
   ```bash
   kubectl apply -f kubernetes/job.yaml

7. **Access the Application**:
   Get the external IP address of the nginx service.
   ```bash
   kubectl get svc

The application can be accessed at the NodePort of the ```nginx-service```. Open a browser and navigate to the external IP and port (e.g., ```http://<node-ip>:<node-port>```).

You should see the message: ```Hello from NFS volume!```