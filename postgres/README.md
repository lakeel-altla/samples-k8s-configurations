# /postgres

Sample configurations for Postgres.

## Prerequirement

See [lakeel-altla/samples-k8s-configurations/setup-samples-mac](../setup-samples-mac) and setup your enviroment for this samples.

## Usage

Deploy the password for `POSTGRES_PASSWORD` as a secret:

```
kubectl create secret generic postgres-secret --namespace=samples --from-literal=password=<your password>
```

Deploy a persistent volume for `PGDATA`:

```
kubectl create -f pv.yaml
```

Deploy a persistent volume claim for it:

```
kubectl create -f pvc.yaml
```

Deploy a service and a statefulset for postgres:

```
kubectl create -f postgres.yaml
```

## Connect

Connect to the virtual machine on which minikube is running:

```
minikube ssh
```

Run psql with docker:

```
docker run -it --rm jbergknoff/postgresql-client postgresql://postgres:@postgres.samples.svc.cluster.local:5432/
```
