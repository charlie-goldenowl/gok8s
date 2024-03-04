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