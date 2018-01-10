# /ingress-nginx-https

Sample configurations to use an ingress with nginx over TLS.

## Prerequirement

See [lakeel-altla/samples-k8s-configurations/dnsmasq-mac](https://github.com/lakeel-altla/samples-k8s-configurations/tree/master/dnsmasq-mac) and setup your local DNS server resolving the domain **minikube.test**.

## Usage

We assume to use Mac here.

We need the latest openssl (1.0.2n) for our Mac.
Update it for Homebrew:

```
brew update
```

or install it:

```
brew install openssl

echo "export PATH=/usr/local/opt/openssl/bin:$PATH" >> ~/.bash_profile
source ~/.bash_profile

which openssl
openssl version
```

Generate our self-signed certificate:

```
mkdir ~/tls
cd ~/tls
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=nginx-tls.minikube.test"
```

Create a namespace `samples` for the registry:

```
kubectl create namespace samples
```

Create a secret for our self-signed certificate:

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
