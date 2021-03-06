# /ingress-nginx-https

Sample configurations to use an ingress with nginx over TLS.

## Usage

Generate your self-signed certificate:

```
mkdir ~/tls
cd ~/tls
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=nginx-tls.minikube.test"
```

Create a secret for your self-signed certificate:

```
kubectl create secret tls nginx-tls --key tls.key --cert tls.crt --namespace=samples
```

Create a service for nginx and an ingress for it:

```
kubectrl create -f nginx.yaml
kubectrl create -f ingress.yaml
```

Test nginx.minikube.test:

```
ping nginx-tls.minikube.test
```

or open https://nginx-tls.minikube.test on your browser.
