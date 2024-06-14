# aks-cluster-setup

1.	Create a Resource Group

2.	Create a Virtual Network and Subnet inside the RG
Note: Enable Public IP bastion under security tab.

3.	Create a Virtual machine with below mentioned configurations
Note: Authentication -> SSH, Public inbound ports: None

Once VM created got to NSG and add new rule in inbound for SSH.

4.	Create Azure Kubernetes service with below mentioned configurations

5.	Connect to the bastion VM with Public IP and SSH key 

6.	Install azure-cli and login with azure creds
```bash
sudo apt-get update
sudo apt-get install azure-cli
sudo az login --service-principal --username <client_ID> --password <client_secret> --tenant <tenant_id>
```

7.  Create empty kubeconfig file
```bash
vi <configfile>
export KUBECONFIG="/home/ubuntu/<configfile>"
sudo az aks get-credentials --resource-group <provision id> --name <provision id> --file <configfile>
```

8.	Install kubectl
```bash
sudo snap install kubectl --classic
```

9.	Execute below command check whether nodes are ready
```bash
kubectl get nodes
```

10.	 Deploy Sample application
```bash
kubectl apply -f deployment.yaml -f service.yaml
```

## How to Install Ingress Nginx Controller

```bash
•	kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml
•	kubectl get pods --namespace ingress-nginx
•	kubectl get service ingress-nginx-controller --namespace=ingress-nginx
```

11.  Create a DNS record set

12.  Create ingress.yaml and deploy
```bash
kubectl apply -f ingress.yaml
```

Try to access the application with DNS with http.

## How to make application secure by applying SSL (https)

```bash
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm upgrade cert-manager jetstack/cert-manager \
    --install \
    --create-namespace \
    --wait \
    --namespace cert-manager
```
This will create three Deployments and some Services and Pods in a new namespace called cert-manager. It also installs various cluster scoped supporting resources such as RBAC roles and Custom Resource Definitions.

You can view some of the resources that have been installed as follows:
```bash
kubectl -n cert-manager get all
kubectl explain Certificate
kubectl explain CertificateRequest
kubectl explain Issuer
```

13.  Create a ClusterIssuer and a Certificate
```bash
kubectl apply -f clusterissuer.yaml -f certificate.yaml
```
