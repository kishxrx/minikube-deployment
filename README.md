# Local Kubernetes Deployment with Minikube

This project demonstrates building a local Kubernetes cluster using Minikube, deploying a sample Nginx application, and managing its lifecycle entirely within a GitHub Codespaces environment. The task also involved solving complex networking challenges specific to this cloud-based development setup.

---

## Tools Used

| Tool | Description |
|------|-------------|
| ![GitHub Codespaces](https://github.githubassets.com/images/modules/logos_page/GitHub-Mark.png) | Cloud-based development environment |
| ![Minikube](https://minikube.sigs.k8s.io/docs/images/logo.svg) | Tool to create and manage a local Kubernetes cluster |
| ![kubectl](https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.png) | Command-line tool to interact with Kubernetes |
| ![Docker](https://www.docker.com/wp-content/uploads/2022/03/Moby-logo.png) | Container runtime used by Minikube |

---

## Objective

The core objective was to deploy, manage, and scale a web application in a local Kubernetes cluster to understand the fundamentals of Kubernetes deployments and services.

---

## Setup Instructions

Instead of committing binaries to the repository, install the tools locally:

### Install kubectl
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client

Install Minikube

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version

Install Docker

Follow the official Docker installation guide: Docker Docs


---

Process & Implementation Steps

1. Cluster Creation
Start a local Kubernetes cluster using Minikube:

minikube start


2. Application Deployment
Create a deployment.yaml to define the Nginx deployment with 2 replicas.


3. Exposing the Application
Create a service.yaml to expose the deployment as a NodePort service.


4. Verification and Scaling
Verify pods:

kubectl get pods

Scale to 4 replicas:

kubectl scale deployment nginx-deployment --replicas=4


5. Log Inspection
Check logs:

kubectl logs <pod-name>




---

Challenges & Troubleshooting

Problem 1: Connection Timeout

Issue: minikube service resulted in ERR_CONNECTION_TIMED_OUT.

Reason: Internal cluster IP not accessible outside Codespaces.

Solution: Forward NodePort manually using Codespaces "Ports" tab.


Problem 2: Persistent 502 Bad Gateway

Issue: Browser returned HTTP 502 even with correct port.

Troubleshooting Steps:

Verified Service endpoints with kubectl describe service.

Tried minikube tunnel (did not work).

Restarted pods: kubectl delete pods -l app=nginx.

Full cluster reset: minikube delete and recreated cluster.


Solution: Direct pod connection with port-forward:

kubectl port-forward <pod-name> 8080:80



---

Deliverables: Screenshots

1. Initial Deployment Verified
kubectl get pods confirms two Nginx pods in Running state.


2. Deployment Scaled to 4 Replicas
kubectl get pods confirms four running pods.


3. Successful Log Verification
Nginx access logs retrieved via kubectl logs after port-forwarding.




---

Outcome & Key Learnings

Successfully deployed and scaled an application in a local Kubernetes cluster.

Learned advanced troubleshooting for network exposure issues in cloud-based environments.

Gained experience using NodePort vs. kubectl port-forward for reliable application access.



---

Notes

Binaries (kubectl and minikube) are not included in this repo due to GitHub file size limits.

Follow the setup instructions above to install these tools locally before running the project.


