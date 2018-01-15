# /nginx-blue-green-2

Sample configurations to do the blue-green deployment with Nginx as an example.
This is based on switching environments with services.

## Usage

Here, we will use nginx-blue-green-2.minikube.test as the blue environment and dev.nginx-blue-green-2.minikube.test as the green one, and switch them with services.

### Deploy an ingress

```
kubectl create -f ingress.yaml
```

### Deploy 1.11 as a blue

```
kubectl create -f nginx-1.11.yaml
kubectl apply -f service-blue-1.11.yaml
```

Now, the blue environment uses Nginx 1.11. You can get that on the error page:

```
curl http://nginx-blue-green-2.minikube.test/test
```

### Deploy 1.12 as a green

```
kubectl create -f nginx-1.12.yaml
kubectl apply -f service-green-1.12.yaml
```

Now, the blue environment uses 1.11 and the green one uses 1.12:

```
curl http://nginx-blue-green-2.minikube.test/test
curl http://dev.nginx-blue-green-2.minikube.test/test
```

### Switch 1.11 to 1.12

```
kubectl apply -f service-blue-1.12.yaml
kubectl delete -f service-green-1.12.yaml
```

Now, the blue environment uses 1.12:

```
curl http://nginx-blue-green-2.minikube.test/test
```

### Deploy 1.13 as a green

```
kubectl create -f nginx-1.13.yaml
kubectl apply -f service-green-1.13.yaml
```

Now, the blue environment uses 1.12 and the green one uses 1.13:

```
curl http://nginx-blue-green-2.minikube.test/test
curl http://dev.nginx-blue-green-2.minikube.test/test
```

### Switch 1.12 to 1.13

```
kubectl apply -f service-blue-1.13.yaml
kubectl delete -f service-green-1.13.yaml
```

Now, the blue environment uses 1.13:

```
curl http://nginx-blue-green-2.minikube.test/test
```

