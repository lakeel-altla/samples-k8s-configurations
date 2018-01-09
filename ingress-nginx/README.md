# /ingress-nginx

Sample configurations to use an ingress with nginx.

# Usage

We assume to use Mac here.

Create a namespace `samples` for the registry:

```
kubectl create namespace samples
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
ping nginx.minikube.test
```

or open http://nginx.minikube.test on your browser.

Note: If you change the dnsmasq configuration, you need to restart it and clear the DNS cache:

```
sudo brew services restart dnsmasq
sudo killall -HUP mDNSResponder
```