**Pre-req:**
# Create a new namespace in EKS:
    - Create a new namespace in the EKS cluster
    $ kubectl create ns three-tier
    - Verify 
    $ kubectl get ns

# Update host to your own host in these files:  
    1. ingress.yaml - host= srishkumar.com
    2. Frontend/deployment.yaml  - http://srishkumar.com/api/tasks


## Steps to Setup a Repository in ArgoCD

1. **Login to ArgoCD UI**
    - Open your web browser and navigate to the ArgoCD UI URL.
    - Enter your username and password to log in.

2. **Navigate to the Repositories Section**
    - In the left-hand menu, click on `Settings`.
    - Under `Settings`, select `Repositories`.

3. **Add a New Repository**
    - Click on the `Connect Repo` button.
    - Fill in the repository details:
      - **Repository URL**: The URL of your Git repository.
      - **Username**: Your Git username (required if repo is private).
      - **Password**: Your Git password or personal access token (required if repo is private).

4. **Test the Connection**
    - Click on the `Test` button to ensure that ArgoCD can connect to your repository.
    - If the test is successful, proceed to the next step. If not, check your credentials and repository URL.

5. **Save the Repository**
    - Click on the `Create` button to save the repository configuration.

6. **Verify the Repository**
    - The repository should now appear in the list of connected repositories.
    - You can click on the repository name to view its details and ensure it is correctly configured.


## Steps to Create a New Application in ArgoCD

1. **Navigate to the Applications Section**
    - In the left-hand menu, click on `Applications`.

2. **Create a New Application**
    - Click on the `New App` button.

3. **Fill in the Application Details**
    - **Application Name**: Enter a name for your application. Generally in lowercase. 
        three-tier-database
    - **Project**: Select the project to which this application belongs.
        default
    - **Sync Policy**: Choose the synchronization policy (e.g., Manual or Automatic).
        Automatic/select self heal checkbox

4. **Specify the Source Repository**
    - **Repository URL**: Enter the URL of your Git repository.
        https://github.com/srish-kumar/ArgoCD-Deploy-EKS.git
    - **Revision**: Specify the branch, tag, or commit to deploy.
        Head
    - **Path**: Enter the path to the directory containing the Kubernetes manifests.
        Kubernetes-Manifests-file/Database

5. **Define the Destination Cluster and Namespace**
    - **Cluster URL**: Select the cluster where the application will be deployed.
        https://kubernetes.default.svc
    - **Namespace**: Enter the namespace in the cluster where the application will be deployed.
        three-tier

6. **Configure Additional Settings (Optional)**
    - **Helm**: If using Helm, provide the necessary Helm chart details.
    - **Kustomize**: If using Kustomize, specify the Kustomize options.
    - **Directory**: If using a directory structure, configure the directory options.

7. **Create the Application**
    - Click on the `Create` button to create the application.

8. **Verify the Application**
    - The application should now appear in the list of applications.
    - You can click on the application name to view its details and ensure it is correctly configured.
    - Verify:
    $ kubectl get all -n three-tier
    $ kubectl get pvc -n three-tier

You have successfully created a new application in the ArgoCD UI.

# Steps to Create a New Application for Backend in ArgoCD
**Database was first deployed so that backend can sync to it**
- Same steps but make these changes.
- **Application Name**: Enter a name for your application. Generally in lowercase. 
        three-tier-backend
- **Path**: Enter the path to the directory containing the Kubernetes manifests.
        Kubernetes-Manifests-file/Backend

# Steps to Create a New Application for Frontend in ArgoCD
- Same steps but make these changes.
- **Application Name**: Enter a name for your application. Generally in lowercase. 
        three-tier-frontend
- **Path**: Enter the path to the directory containing the Kubernetes manifests.
        Kubernetes-Manifests-file/Frontend

# Steps to Create a New Application for Ingress in ArgoCD
** Application needs ingress deployment to be accessible from outside, as service type is - ClusterIP.**
** Ingress yaml is at the root of manifest folder.**
- Same steps but make these changes.
- **Application Name**: Enter a name for your application. Generally in lowercase. 
        three-tier-ingress
- **Path**: Enter the path to the directory containing the Kubernetes manifests.
        Kubernetes-Manifests-file/
- Validate - Once Ingress application is deployed, it will create an Application Load Balancer. Validate that a new
  loadbalancer is created and is in active state after the ingress deployment.


## ADD DNS record in Route53
- Validate that a loadbalancer is created and is in active state after the ingress deployment.
- DNS name will not be accesible directly. We need to map the DNS to a Route53 record.

# Steps to Add a Record in Route53

1. **Select Hosted Zones**
    - In the Route53 dashboard, click on `Hosted zones` in the left-hand menu.

2. **Choose Your Domain**
    - Select the domain for which you want to add a new DNS record.

3. **Create Record Set**
    - Click on the `Create record set` button.

4. **Fill in the Record Details**
    - **Name**: Enter the subdomain or leave it blank for the root domain.
        Leave blank as no subdomain is used
    - **Type**: Select the record type (e.g., A, CNAME).
        Record Type A, as we will be using AWS load balancer
    - **Alias**: If you are creating an alias record, select `Yes` and choose the appropriate alias target.
        Select alias
        Choose endpoint - Alias to Application and Classic Load Balancer
        Choose Region - us-east-1
        Choose load balancer from the dropdown - k8s.three-tie-mainalb
    - **Value**: Enter the value for the record (e.g., the IP address or DNS name of the load balancer). - NA
    - **TTL (Time to Live)**: Set the TTL value (e.g., 300 seconds). -NA

5. **Save the Record**
    - Click on the `Create` button to save the new DNS record.

6. **Verify the Record**
    - Use a DNS lookup tool to verify that the new record has been propagated and is resolving correctly.

You have successfully added a new DNS record in Route53.
