# /ingress-nginx

Sample configurations to use an ingress with nginx.

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
