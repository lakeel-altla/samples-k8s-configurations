# /ingress-nginx

Sample configurations to use an ingress with nginx.

## Prerequirement

See [lakeel-altla/samples-k8s-configurations/setup-samples-mac](../setup-samples-mac) and setup your enviroment for this samples.

## Usage

Create a service for nginx and an ingress for it:

```
kubectrl create -f nginx.yaml
kubectrl create -f ingress.yaml
```

Test nginx.minikube.test:

```
ping nginx.minikube.test
```

or open http://nginx.minikube.test on your browser.
