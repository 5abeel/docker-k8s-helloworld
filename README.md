# docker-k8s-helloworld

A 'Hello World' example of building containers and deploying in Docker runtime & k8s (minikube)

Pre-req:
* Docker 
* Minikube

## Build Docker Container

Build the Docker container and run it as 'hello-app'

```
docker build --tag hello .
docker run -d --name hello-app -p 5000:5000 hello
```

Confirm status with
```
docker images
docker ps
```

Test with browser or curl: `localhost:5000`


## Build & Deploy on Minikube

1. Start Minikube

```
minikube start
```
2. Set Docker env
```
eval $(minikube docker-env)
```
3. Build image again
```
docker build --tag hello .
```
4. Enable ingress in minikube

Confirm that the ingress pod is running in 'kube-system' namespace.

```
minikube addons enable ingress
kubectl get pods -n kube-system
```

5. Run the deployment and create a service for nodeport 5000

```
kubectl run hello --image=hello:latest --image-pull-policy=Never --port=5000
kubectl expose deployment hello --type=NodePort --port=5000
```

6. Get IP address to reach the pod via minikube url
```
minikube service hello --url
```

Test with browser or curl: `<ip:port>` from the last command above