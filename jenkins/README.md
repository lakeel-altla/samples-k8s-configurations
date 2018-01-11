# /jenkins

Sample configurations of jenkins with autometed setup.

## Usage

### Fix an issue with kube-dns (Minikube v0.24.1)

Pulling images on minikube v0.24.1 fails to resolve the domain .cluster.local.

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

### Prepare the private registry

See [lakell-altla/samples-k8s-configurations/registry](../registry).

### Build and push the image jenkins-with-plugins

Clone and build the image [lakell-altla/samples-dockerfile/jenkins-with-plugins](../registry):

```
docker build -t jenkins-with-plugins .
```

Push it:

```
docker tag jenins-with-plugin:latest registry.minikube.test/sample/jenkins-with-plugins:latest
docker push registry.minikube.test/sample/jenkins-with-plugins:latest
```

### Deploy

Add your account for Jenkins as secrets:

```
kubectl create secret generic jenkins-account --from-literal=username=<your username> --from-literal=password=<your password>
```

Generate your self-signed certificate:

```
mkdir ~/tls
cd ~/tls
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout tls.key -out tls.crt -subj "/CN=jenkins.minikube.test"
```

Create a secret for your self-signed certificate:

```
kubectl -n samples create secret tls jenkins-tls --key tls.key --cert tls.crt
```

```
kubectl create -f pv.yaml
kubectl create -f pvc.yaml
kubectl create -f master.yaml
kubectl create -f ingress.yaml
```

### Access

Open https://jenkins.minikube.test on your browser and login your account.
