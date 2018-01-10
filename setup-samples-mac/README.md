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
