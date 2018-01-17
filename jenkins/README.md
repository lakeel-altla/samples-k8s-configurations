# /jenkins

Sample configurations of jenkins with autometed setup.

## Usage

### Prepare the private registry

See [lakell-altla/samples-k8s-configurations/registry](../registry).

### Build and push the image jenkins with automated setup

Clone and build the image [lakell-altla/samples-dockerfile/jenkins](https://github.com/lakeel-altla/samples-dockerfile/tree/master/jenkins):

```
docker build -t jenkins .
```

Push it:

```
docker tag jenkins:latest registry.minikube.test/sample/jenkins:latest
docker push registry.minikube.test/sample/jenkins:latest
```

### Deploy

Add your account for Jenkins as secrets:

```
kubectl create secret generic jenkins-account --namespace=samples --from-literal=username=<your username> --from-literal=password=<your password>
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
