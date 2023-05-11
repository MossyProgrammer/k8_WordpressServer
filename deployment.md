### Deploying AKS Cluster (using PowerShell):

```
New-AzAksCluster -ResourceGroupName [enter_ResourceGroup] -Name [enter_AKSCluster_name] -NodeCount 1 -GenerateSshKey -WorkspaceResourceId <WORKSPACE_RESOURCE_ID>

Install-AzAksKubectl

Import-AzAksCredential -ResourceGroupName [enter_ResourceGroup] -Name [enter_AKSCluster_name]
```
Verify connection with: `kubectl get nodes`

### Deploying AKS Cluster (using Azure CLI):
```
az aks create -g [enter_ResourceGroup] -n [enter_AKSCluster_name] --enable-managed-identity --node-count 1 --enable-addons monitoring --enable-msi-auth-for-monitoring  --generate-ssh-keys

az aks install-cli
az aks get-credentials --resource-group [enter_ResourceGroup] --name [enter_AKSCluster_name]
```


### Deploying Wordpress and MySQL container instances:
Connect to cluster

```
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f mysql-deployment.yaml
```
**OR**
```
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=ENTER_YOUR_PASSWORD
EOF

curl -LO https://k8s.io/examples/application/wordpress/mysql-deployment.yaml
curl -LO https://k8s.io/examples/application/wordpress/wordpress-deployment.yaml
  
cat <<EOF >>./kustomization.yaml
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml
EOF

kubectl apply -k ./
```

