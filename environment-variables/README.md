
# environment-variables

Sample configurations to pass environment variables to docker containers.

## Prerequirement

See [lakeel-altla/samples-k8s-configurations/setup-samples-mac](../setup-samples-mac) and setup your enviroment for this samples.

## Usage

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