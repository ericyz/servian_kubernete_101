### Install kubectl
```
brew install kubernetes-cli
```
Verify the installation
```
kubectl version
```
### Install & Run Minikube
Install in MAC OS
```
brew cask install minikube
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 \
  && chmod +x minikube
sudo mv minikube /usr/local/bin
```
Run minikube
```
minikube start
```

Verify you are connected with Minikube
```
kubectl config current-context
```

Reference: https://kubernetes.io/docs/setup/learning-environment/minikube/

### Deploy the Bookinfo apps
Deploy the reviews microservice
```
cp k8s/reviews/deployment.v1.yaml k8s/reviews/deployment.yaml
kubectl apply -f k8s/reviews/deployment.yaml
kubectl apply -f k8s/reviews/service.yaml
```
Deploy the rating microservice
```
kubectl apply -f k8s/ratings/deployment.yaml
kubectl apply -f k8s/ratings/service.yaml
```
Deploy the detail microservice
```
kubectl apply -f k8s/detail/deployment.yaml
kubectl apply -f k8s/detail/service.yaml
```
Deploy the productpage microservice
kubectl apply -f k8s/productpage/deployment.yaml
kubectl apply -f k8s/productpage/service.yaml

### Access the deployed app
Get the ip address it runs on minikube
```
minikube service productpage --url
```
Access the website on http://{host_url}/productpage

### Rolling Update the Review microservice
V2 - Bad Deployment
```
cp k8s/reviews/deployment.v2.yaml k8s/reviews/deployment.yaml
kubectl apply -f k8s/reviews/deployment.yaml
```

Rollback
```
last_version=$(kubectl rollout history deployment/reviews | tail  -n 3 | cut -c 1 | head -n 1)
kubectl rollout undo deployment reviews --to-revision=${last_version}
```
