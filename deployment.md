### Deploying AKS Cluster (using Azure CLI):
```
export RESOURCE_GROUP_NAME=re-lee
export AKS_NAME=autotest_clusterccfinal
az aks create \
	--resource-group $RESOURCE_GROUP_NAME \
	--name $AKS_NAME \
	--os-sku Ubuntu \
	--enable-managed-identity \
	--node-count 1 \
	--enable-addons monitoring \
	--generate-ssh-keys \
	--location eastus\
	--enable-cluster-autoscaler \
	--max-count 5 \
	--min-count 1 \
	--network-plugin kubenet \
	--load-balancer-sku basic \
	--tier free
az aks open-port --port 80 --resource-group $RESOURCE_GROUP_NAME --name $AKS_NAME
az aks install-cli
az aks get-credentials --resource-group [enter_ResourceGroup] --name [enter_AKSCluster_name]
```
Verify connection with: `kubectl get nodes`
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
Get External IP by `kubectl get service wordpress --watch`
