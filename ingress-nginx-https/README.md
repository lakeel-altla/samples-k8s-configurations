# /ingress-nginx

Sample configurations to use an ingress with nginx over TLS.

# Usage

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

Install dnsmasq:

```
brew install dnsmasq
```

Create `/usr/local/etc/dnsmasq.conf`:

```
vi /usr/local/etc/dnsmasq.conf
```
```
bind-interfaces
listen-address=127.0.0.1
address=/minikube.test/`minikube ip`
```

or:

```
echo bind-interfaces > /usr/local/etc/dnsmasq.conf
echo listen-address=127.0.0.1 >> /usr/local/etc/dnsmasq.conf
echo address=/minikube.test/`minikube ip` >> /usr/local/etc/dnsmasq.conf
```

Create `/etc/resolver/minikube.test` as root:

```
sudo mkdir -p /etc/resolver/
sudo vi /etc/resolver/minikube.test
```
```
nameserver 127.0.0.1
```

or:

```
sudo mkdir -p /etc/resolver/
sudo bash -c "echo 'nameserver 127.0.0.1' > /etc/resolver/minikube.test"
```

Start dnsmasq:

```
sudo brew services start dnsmasq
```

Test nginx.minikube.test:

```
ping nginx-tls.minikube.test
```

or open https://nginx-tls.minikube.test on your browser.

Note: If you change the dnsmasq configuration, you need to restart it and clear the DNS cache:

```
sudo brew services restart dnsmasq
sudo killall -HUP mDNSResponder
```