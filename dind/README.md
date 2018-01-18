# /dind

Sample configuration for DinD (Docker in Docker).

## Usage

Deploy the dind daemon:

```
kubectl create -f dind.yaml
```

Deploy a container to run `docker run hello-world`:

```
kubectl create -f docker-cmd.yaml
```

Check its container's logs:

```
kubectl -n samples logs docker-cmd
```
