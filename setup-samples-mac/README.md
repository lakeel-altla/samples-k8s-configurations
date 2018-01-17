# /setup-samples-mac

How to setup for this repository on Mac.

## Create a new namespace

Create a namespace `samples` for samples:

```
kubectl create namespace samples
```

## Setup openssl

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

## Setup dnsmasq

This repository uses the domain **minikube.test**.

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

### Note

If you change the dnsmasq configuration, you need to restart it and clear the DNS cache:

```
sudo brew services restart dnsmasq
sudo killall -HUP mDNSResponder
```

## Fix an issue with kube-dns (Minikube v0.24.1)

Minikube v0.24.1 fails to resolve the domain .cluster.local.

- [issues/1674](https://github.com/kubernetes/minikube/issues/1674)
- [issues/2162](https://github.com/kubernetes/minikube/issues/2162)

So, update your nameserver in the minikube to fix this issue.

On minikube host, open `/etc/systemd/resolved.conf`:

```
sudo vi /etc/systemd/resolved.conf
```

Set the ip addess of your kube-dns into `DNS`. For example update as below:

```
DNS=10.96.0.10
```

Restart systemd-resolved:

```
sudo systemctl restart systemd-resolved
```

Note: This setting will be lost when restarting the minikube.