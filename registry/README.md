# /registry

Sample configurations to use a private docker registry between virtual clusters.

## Usage

### Setup the registry

Create a namespace `sample-registry` for the registry:

```
kubectl create namespace sample-registry
```

Create a persistent volume for the registry to store docker images:

```
kubectl create -f registry/pv.yaml
```

Create a persistent volume claim for the registry to use the persistent volume:

```
kubectl create -f registry/pvc.yaml
```

Create a service running the registry as statefulset:

```
kubectl create -f registry/statefulset.yaml
```

### Setup a client environment for the registry


Create a namespace `sample-registry-client` for the client enviroment:

```
kubectl create namespace sample-registry-client
```

Create the client enviroment:

```
kubectl create -f client/pod.yaml
```

### Push and pull a image

Connect to the client enviroment:

```
kubectl --namespace=sample-registry-client exec -it registry-client /bin/sh
```

Pull a test image `hello-world`:

```
docker pull hello-world
```

Set a tag into the image to push the registry `registry.sample-registry:5000`:

```
docker tag hello-world registry.sample-registry:5000/sample/hello-world:latest
```

Push the image to the registry `registry.sample-registry:5000`:

```
docker push registry.sample-registry:5000/sample/hello-world:latest
```

Remove the image:

```
docker rmi registry.sample-registry:5000/sample/hello-world:latest
```

Pull the image from the registry `registry.sample-registry:5000`:

```
docker pull registry.sample-registry:5000/sample/hello-world
```

## (Optional) Deploy kwk/docker-registry-frontend

Create a service for [kwk/docker-registry-frontend](https://github.com/kwk/docker-registry-frontend):

```
kubectl create -f registry/frontend.yaml
```

Access to it (a sample with minikube here):

```
minikube -n=sample-registry service frontend
```
