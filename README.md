# ArgoCD-Deploy-EKS
Deployment of a tree tier application on EKS cluster using Terraform and GitOps tool ArgoCD.
```
Infrastructure-as-code:
AWS Infrastructure setup using Terraform:
Networking setup using Terraform (VPC, subnet, security groups etc.)
IAM roles setup
Kubernetes (EKS) Cluster creation and setup
Complete setup using Jenkins/GitHub pipeline.
Deploying Ingress Controller
Deployment of Ingress Controller, using AWS CLI and Helm charts 
Service account setup
Setting up load balancer for EKS cluster

GitOps (For Continuous Delivery):
GitHub pipeline to automate
Build the application images, 
Push the new image to private repository (ECR) and 
Update K8s manifest files in GitHub repo (simple manifest or helm chart).
ArgoCD to deploy new K8 changes
Use ArgoCD to continuously monitor the manifest files/Helm chart, in GitHub repository
Pick the changes and automatically deploy on the EKS cluster.
DNS name mapping using Route53 to access the application.
```

**Step 1: Build application, push image to ECR and update deployment manifest file.**

Points to note:
1. Settings  -> Actions -> General -> Workflow permissions
   Select Read and write permissions, which is not selected by default.
![image](https://github.com/user-attachments/assets/9a0c81ca-4a57-4058-bf21-1722833d6f1c)

2. Set following Repository Secrets:
  ```
 AWS_ACCOUNT_ID
 AWS_ACCESS_KEY_ID
 AWS_SECRET_ACCESS_KEY
 AWS_REGION
```

3. To run the frontend or backend pipeline, create tag staring with frontend or backend (frontend* or backend *):
 ## Commands to Run:
 ```
   git add .
   git commit -m "Commit message"
   git push origin main
   git tag -a <tag> -m "message"
   git push origin <tag>
```




   







