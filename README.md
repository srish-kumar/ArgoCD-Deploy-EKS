# ArgoCD-Deploy-EKS
Deployment of a tree tier application on EKS cluster using GitOps tool ArgoCD.

Points to note:
1. Settings  -> Actions -> General -> Workflow permissions
   Select Read and write permissions, which is not selected by default.
![image](https://github.com/user-attachments/assets/9a0c81ca-4a57-4058-bf21-1722833d6f1c)

2. Set following Repository Secrets:
   AWS_ACCOUNT_ID
   AWS_ACCESS_KEY_ID
   AWS_SECRET_ACCESS_KEY
   AWS_REGION

3. To run the frontend or backend pipeline, create tag staring with frontend or backend (frontend* or backend *):
 ## Commands to Run:
 ```
   git add .
   git commit -m "Commit message"
   git push origin main
   git tag -a <tag> -m "message"
   git push origin <tag>
   







