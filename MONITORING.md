# Documentation for Installing Prometheus and Grafana using Helm Stable

Pre-req: Install Helm Stable
**Add Helm Stable Repository**:
    - Run `helm repo add stable https://charts.helm.sh/stable` to add the stable repository.

**Steps to intall Prometheus and Grafana using Helm:**

1. **Add and Install Prometheus Community Repository**:
    - `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
    - To verify run `helm repo list`
    - `helm repo update`
    - To install the repo run `helm install prometheus prometheus-community/prometheus`
    - Prometheous gets deployed in the default namespace. To verify run `kubectl get all`

2. **Add and Install Grafana Repository**:
    - `helm repo add grafana https://grafana.github.io/helm-charts`
    - To verify run `helm repo list`
    - `helm repo update`
    - `helm install grafana grafana/grafana`
    - Grafana gets deployed in the default namespace. To verify run `kubectl get all`

3. **Modify Prometheus and Grafana service Type**
    - `kubectl get svc -n default` - Prometheus and Grafana services are of type ClusterIP.
    -  To access Prometheus and Grafana consoles from outside of the cluster, change the Service type 
       from ClusterIP to LoadBalancer.
    
    **Modify Prometheus service type:**
    - `kubectl edit svc prometheus-server`
    - Edit and change the service type to "LoadBalancer"
    - Verify - `kubectl get svc prometheus-server`. 
      LoadBalancers DNS name (elb) under External-IP should be present. Varify LB from AWS console.
    - Note - we can also create ingress for acheiveing the same.
    - Access Prometheus using the External IP. It may take sometime to come up.
    - To see list of resources (Targets) scrapped by Prometheus goto Status -> Targets

    **Modify Grafana service type:**
    - `kubectl edit svc grafana`
    - Edit and change the service type to "LoadBalancer"
    - Verify - `kubectl get svc prometheus-server`.
      LoadBalancers DNS name (elb) under External-IP should be present. Varify LB from AWS console.
    - Note - we can also create ingress for acheiveing the same.

4. **Get Grafana secrets**
    - `kubectl get secrets`
    - Edit the secret for Grafana `kubectl edit secrets grafana`
    - Copy the admin password and username under data section. These are encoded.
    - Decode the password `echo "<password base64_encoded_string>" | base64 --decode
    - Decode the username `echo "<username base64_encoded_string>" | base64 --decode
    - Decoded    admin.
    - Other way to get the Grafana secret is to run jasonpath command-
      `kubectl get secrets --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode; echo`

5. **Create datasource in Grafana**
    - Open the Grafana Load balancer DNS in a browser to access the Grafana dashboard.
    - Login to Grafana using username and password.
    - Navigate to Connections -> Data Sources.
    - Click on "Add data source".
    - Select "Prometheus" from the list of data sources.
    - In the HTTP URL field, enter the Prometheus server URL (DNS name of the Load balancer 
      should strat with http://).
    - Click "Save & Test" to verify the connection.
    - If connection is successfull message displayed "Successfully queried the Prometheus API"

6. **Create a dashboard to visualize our Kubernetes Cluster Logs**
    - Select "Dashboards", on the left sidebar.
    - Click on New --> Import
    - Use pre-configured prometheus dashboards by providing Ids under - 
      Find and import dashboards for common applications - IDs - 6417, 17375 , 315
    - Provide ID = 6417 and click on Load. (this dashboard is more focussed on monitoring cluster resources)
    - Select the Prometheus data source (default that was created earlier) from the dropdown.
    - Click on Import.
    - Configured dashboard will appear. 
    - We can filter the dashboard for specific namespace or nodes from top.
    - Similarly configure another dashboard with Id = 17375 (this is more focussed on monitoring memory and cpu usage)

    **We can also manually configure the Dashboard for Prometheus:**
    - In the "Query" section, select "Prometheus" as the data source.
    - Enter a Prometheus query to fetch the Kubernetes logs, for example:
    ```
    {job="kubernetes-logs"}
    ```
    - Customize the visualization options as needed.
    - Click "Apply" to save the panel.
    - Repeat the above steps to add more panels to the dashboard.
    - Once all panels are added, click "Save dashboard" at the top.
    - Provide a name for the dashboard and click "Save".



    







