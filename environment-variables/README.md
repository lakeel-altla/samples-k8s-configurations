
# environment-variables

Sample configurations to pass environment variables to docker containers.

## Usage

Create a namespace `samples` used by this configuration:

```
kubectl create namespace samples
```

Create and run a pod passed environment variables and printing them:

```
kubectl create -f pod.yaml
```

Check logs of the pod:

```
kubectl --namespace=samples logs environment-variables
```

This log contains following environment variables passed by pod.yaml:

```
...
SAMPLE_HELLO_WORLD=Hello world
SAMPLE_PATH=/var/log
...
```