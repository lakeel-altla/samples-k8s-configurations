# /nginx-blue-green

Sample configurations to do the blue-green deployment with Nginx as an example.

## Usage

Here, we will use nginx-blue-green.minikube.test as the blue environment and dev.nginx-blue-green.minikube.test as the green one, and switch them with the ingress.

### Deploy Nginx 1.11

Create a depolyment and an ingress for Nginx 1.11:

```
kubectl create -f nginx-1.11.yaml
kubectl create -f ingress-1-11.yaml
```

Now, the blue environment uses Nginx 1.11. You can get that on the error page:

```
curl http://nginx-blue-green.minikube.test/test
```

### Deoloy Nginx 1.12

Create a deployment for Nginx 1.12 as a green environment.

```
kubectl create -f nginx-1.12.yaml
kubectl apply -f ingress-1-11-and-1-12.yaml
```

Now, the blue environment uses Nginx 1.11 and the green one uses 1.12:

```
curl http://nginx-blue-green.minikube.test/test
curl http://dev.nginx-blue-green.minikube.test/test
```

### Swtich the blue environment to the green one

Switch Swtich the blue environment to the green one:

```
kubectl apply -f ingress-1-12.yaml
```

Now, the blue environment uses Nginx 1.12 and the green one is close:

```
curl http://nginx-blue-green.minikube.test/test
curl http://dev.nginx-blue-green.minikube.test/test
```

### Remove the previous blue environment

Remove Nginx 1.11:

```
kubectl delete -f nginx-1.11.yaml
```

### Deoloy Nginx 1.13

Create a deployment for Nginx 1.13 as a new green environment.

```
kubectl create -f nginx-1.13.yaml
kubectl apply -f ingress-1-12-and-1-13.yaml
```

Now, the blue environment uses Nginx 1.12 and the green one uses 1.13:

```
curl http://nginx-blue-green.minikube.test/test
curl http://dev.nginx-blue-green.minikube.test/test
```

### Swtich the blue environment to the green one

Switch Swtich the blue environment to the green one:

```
kubectl apply -f ingress-1-13.yaml
```

Now, the blue environment uses Nginx 1.13 and the green one is close:

```
curl http://nginx-blue-green.minikube.test/test
curl http://dev.nginx-blue-green.minikube.test/test
```

### Remove the previous blue environment

Remove Nginx 1.12:

```
kubectl delete -f nginx-1.12.yaml
```
