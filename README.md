### 1. build the docker image

```shell
docker build -t nmvuong92/gok8s .
docker run -it -p 3000:3000 nmvuong92/gok8s
```

### 2. run kube dashboard
https://stackoverflow.com/a/75160251

```shell
kubectl apply -f create-service-cccount.yaml
kubectl apply -f create-cluster-role-binding.yaml
kubectl -n kubernetes-dashboard create token admin-user
```

### configure pod container (private docker hub)
```shell
kubectl create secret generic regcred \
    --from-file=.dockerconfigjson=/Users/vuong/.docker/config.json \
    --type=kubernetes.io/dockerconfigjson
```


### get pod info (error)
```shell
kubectl get pods -A
kubectl describe pod gok8s-589c47c44-z6kf4
```


### forward port
```shell
kubectl get pods -A
kubectl port-forward gok8s-86b79cf757-nb729 8080:8080
```

### scale pods
```shell
kubectl scale --replicas=4 deployment/gok8s
```


### make Deployment to update image
```shell
kubectl rollout restart deployment/gok8s
```

update config in deployment.yaml
```shell
imagePullPolicy: Always
```


### get service url
```shell
minikube service gok8s-service --url
```

### Kubernetes proxy
```shell
kubectl proxy --port=8080
```
When would you use this?
There are a few scenarios where you would use the Kubernetes proxy to access your services.

- Debugging your services, or connecting to them directly from your laptop for some reason
- Allowing internal traffic, displaying internal dashboards, etc.
### Nodeport
When would you use this?
- You can only have one service per port
- You can only use ports 30000–32767
- If your Node/VM IP address change, you need to deal with that

### LoadBalancer

If you want to directly expose a service, this is the default method.
All traffic on the port you specify will be forwarded to the service.
There is no filtering, no routing, etc. This means you can send almost any kind of traffic to it, 
like HTTP, TCP, UDP, Websockets, gRPC, or whatever.

The big downside is that each service you expose with a LoadBalancer will get its own IP address, 
and you have to pay for a LoadBalancer per exposed service, which can get expensive!

### Ingress

Ingress is probably the most powerful way to expose your services, 
but can also be the most complicated. 
There are many types of Ingress controllers, 
from the Google Cloud Load Balancer, Nginx, Contour, Istio, and more. 
There are also plugins for Ingress controllers, like the cert-manager, 
that can automatically provision SSL certificates for your services.
https://github.com/cert-manager/cert-manager