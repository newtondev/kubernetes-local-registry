## 1. Create a registry on minikube

Create a registry (a replication-controller and a service) and create a proxy to make sure the minikube VM's 5000 is proxied to the registry service's 5000.

```
kubectl create -f kube-registry.yaml
```

minikube ssh && curl localhost:5000 should work and give a response from the docker registry.

## 2. Map the host port 5000 to minikube registry pod

```
kubectl port-forward --namespace kube-system \
$(kubectl get po -n kube-system | grep kube-registry-v0 | \
awk '{print $1;}') 5000:5000
```

From the host curl localhost:5000 should return a valid response.

## 3. Non-linux peoples: Map docker-machine's 5000 to the host's 5000.

```
ssh -i ~/.docker/machine/machines/default/id_rsa \
-R 5000:localhost:5000 \
docker@$(docker-machine ip)
```
