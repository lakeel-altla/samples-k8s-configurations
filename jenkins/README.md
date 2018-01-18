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

Deploy your account for Jenkins as secrets:

```
kubectl create secret generic jenkins-account --namespace=samples --from-literal=username=YOUR_USERNAME --from-literal=password=YOUR_PASSWORD
```

Deploy Jenkins:

```
kubectl create -f .
```

### Access

Open https://jenkins.minikube.test on your browser and login your account.
