
# environment-variables

A sample configuration to pass environment variables to docker containers.

## Usage

Create a namespace `samples` used by this configuration:

```
kubectl create namespace samples
```

Create and run a pod `sample-environment-variables` passed environment variables and printing them:

```
kubectl create -f pod.yaml
```

Check a log of the pod `sample-environment-variables`:

```
kubectl --namespace=samples logs sample-environment-variables
```

This log contains following environment variables passed by pod.yaml:

```
...
SAMPLE_HELLO_WORLD=Hello world
SAMPLE_PATH=/var/log
...
```