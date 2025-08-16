Local Kubernetes Deployment with Minikube

Project Overview

This project demonstrates the process of building a local Kubernetes cluster using Minikube, deploying a sample Nginx application, and managing its lifecycle. All tasks were performed within a GitHub Codespaces environment.

A significant portion of this project involved diagnosing and resolving complex networking challenges specific to the cloud-based development environment, providing a deep learning experience in Kubernetes operations and troubleshooting.


---

Project Goal

The core objective of this project was to deploy, manage, and scale a web application in a local Kubernetes cluster to gain hands-on experience with:

1. Kubernetes cluster setup and management.


2. Application deployment using YAML manifests.


3. Exposing applications to external network traffic.


4. Scaling deployments and inspecting logs.


5. Troubleshooting advanced networking issues in cloud-based environments.




---

Technologies Used

GitHub Codespaces – cloud-based development environment.

Minikube – for creating and managing a local Kubernetes cluster.

kubectl – command-line tool for interacting with Kubernetes API.

Docker – container runtime used by Minikube.



---

Process & Implementation Steps

The deployment followed these key steps:

1. Cluster Creation
Installed Minikube and kubectl in the Codespace, then launched a Kubernetes cluster using:

minikube start


2. Application Deployment
Created a deployment.yaml file defining the desired state for the application. Configured it to run two replicas of the nginx:1.14.2 container image:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80


3. Exposing the Application
Created a service.yaml file to expose the Nginx deployment using a NodePort service:

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30080


4. Verification and Scaling
Verified the deployment:

kubectl get pods

Scaled the deployment to 4 replicas:

kubectl scale deployment nginx-deployment --replicas=4


5. Log Inspection
Inspected logs of a running pod:

kubectl logs <pod-name>




---

Challenges & Troubleshooting Journey

Challenge 1: Connection Timeout

Issue: Accessing the service using minikube service resulted in ERR_CONNECTION_TIMED_OUT.

Reason: Minikube provided an internal cluster IP (192.168.x.x) not accessible from the Codespaces virtual network.

Solution: Forwarded the correct NodePort manually using the Codespaces “Ports” tab.


Challenge 2: Persistent 502 Bad Gateway

Issue: Browser returned HTTP 502 even after correct port forwarding.

Troubleshooting Steps:

Verified service endpoints (kubectl describe service).

Attempted minikube tunnel.

Restarted pods (kubectl delete pods -l app=nginx).

Reset cluster (minikube delete and recreated).


Solution: Network incompatibility between Minikube service routing and Codespaces. Resolved by using kubectl port-forward directly to a pod:

kubectl port-forward <pod-name> 8080:80



---

Key Deliverables

1. Initial Deployment Verified
Verified that the two Nginx pods were created and running.


2. Deployment Scaled to 4 Replicas
Confirmed successful scaling of the deployment.


3. Successful Log Verification
Access logs verified using kubectl port-forward.




---

Outcome & Key Learnings

This project successfully demonstrated Kubernetes fundamentals while providing hands-on experience in:

Cluster creation and management with Minikube.

Deployment scaling and service exposure strategies.

Advanced troubleshooting in cloud-based environments.

Understanding the difference between NodePort and port-forward access for services.



---
