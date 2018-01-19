# /jenkins-docker

Sample configuration for Jenkins with Docker.

## Usage

### Prepare the private registry

See [lakell-altla/samples-k8s-configurations/registry](../registry).

### Build and push the image jenkins with automated setup

Clone and build the image [lakell-altla/samples-dockerfile/jenkins-docker](https://github.com/lakeel-altla/samples-dockerfile/tree/master/jenkins-docker):

```
docker build -t jenkins-docker .
```

Push it:

```
docker tag jenkins-docker:latest registry.minikube.test/sample/jenkins-docker:latest
docker push registry.minikube.test/sample/jenkins-docker:latest
```

### Deploy

Deploy your account for Jenkins as secrets:

```
kubectl create secret generic jenkins-docker-account --namespace=samples --from-literal=username=YOUR_USERNAME --from-literal=password=YOUR_PASSWORD
```

Deploy Jenkins:

```
kubectl create -f .
```

### Test

Open https://jenkins-docker.minikube.test on your browser and login your account.

Create and run a pipeline with the following script:

```
pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```
