### Deploying AKS Cluster (using Azure CLI):
To deploy the intial AKS cluster, run the following commands in Azure CLI:
(If you want to deploy from an ARM template, reference attached template)
```
export RESOURCE_GROUP_NAME=[enter_ResourceGroup]
export AKS_NAME=[enter_AKSCluster_name]

(if needed: az group create --name [enter_ResourceGroup] --location eastus)

az aks create \
	--resource-group $RESOURCE_GROUP_NAME \
	--name $AKS_NAME \
	--os-sku Ubuntu \
	--enable-managed-identity \
	--node-count 1 \
	--enable-addons monitoring \
	--generate-ssh-keys \
	--enable-cluster-autoscaler \
	--max-count 5 \
	--min-count 1 \
	--network-plugin kubenet \
	--load-balancer-sku basic \
	--tier free
```
Install kubectl and connect to the cluster:
```
az aks install-cli
az account set --subscription [enter_subscriptionID]
az aks get-credentials --resource-group [enter_ResourceGroup] --name [enter_AKSCluster_name]
```
Verify connection with: `kubectl get nodes`

### Deploying Wordpress and MySQL container instances:
Generate secrets for the MySQL container:
```
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=ENTER_YOUR_PASSWORD
EOF
```
Deploy Wordpress and MySQL using deployment.yaml files:
```
kubectl apply -f wordpress-deployment.yaml
kubectl apply -f mysql-deployment.yaml
```
**OR**
```
curl -LO https://k8s.io/examples/application/wordpress/mysql-deployment.yaml
curl -LO https://k8s.io/examples/application/wordpress/wordpress-deployment.yaml
  
cat <<EOF >>./kustomization.yaml
resources:
  - mysql-deployment.yaml
  - wordpress-deployment.yaml
EOF

kubectl apply -k ./
```
Get External IP to Wordpress: `kubectl get service wordpress --watch`

Login and configure Wordpress from the Wordpress UI.

Wordpress and MySQL code referenced from [kubernetes.io](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/)
