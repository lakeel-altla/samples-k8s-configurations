# /registry

Sample configurations to use a private docker registry between virtual clusters.

## Usage

### Setup the registry

Create a namespace `samples` for the registry:

```
kubectl create namespace samples
```

Create a persistent volume for the registry to store docker images:

```
kubectl create -f pv.yaml
```

Create a persistent volume claim for the registry to use the persistent volume:

```
kubectl create -f pvc.yaml
```

Create a service of the registry:

```
kubectl create -f registry.yaml
```

### Setup a client environment for the registry

Create the client enviroment:

```
kubectl create -f client.yaml
```

### Push and pull a image

Connect to the client enviroment:

```
kubectl --namespace=samples exec -it registry-client /bin/sh
```

Pull a test image `hello-world`:

```
docker pull hello-world
```

Set a tag into the image:

```
docker tag hello-world registry:5000/sample/hello-world:latest
```

Push the image to the registry:

```
docker push registry:5000/sample/hello-world:latest
```

Remove the image:

```
docker rmi registry:5000/sample/hello-world:latest
```

Pull the image from the registry:

```
docker pull registry:5000/sample/hello-world
```

## (Optional) Deploy kwk/docker-registry-frontend

Create a service for [kwk/docker-registry-frontend](https://github.com/kwk/docker-registry-frontend):

```
kubectl create -f frontend.yaml
```

Access to it (a sample with minikube here):

```
minikube -n=samples service registry-frontend
```
