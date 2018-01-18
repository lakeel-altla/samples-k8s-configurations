# /mysql

Sample configurations for MySQL.

## Usage

Deploy the password for `MYSQL_ROOT_PASSWORD` as a secret:

```
kubectl create secret generic mysql-secret --namespace=samples --from-literal=password=YOUR_PASSWORD
```

Deploy a persistent volume for `/var/lib/mysql`:

```
kubectl create -f pv.yaml
```

Deploy a persistent volume claim for it:

```
kubectl create -f pvc.yaml
```

Deploy a service and a statefulset for mysql:

```
kubectl create -f mysql.yaml
```

## Connect

Connect to the virtual machine on which minikube is running:

```
minikube ssh
```

Run `mysql` with docker:

```
docker run -it --rm mysql mysql -hmysql.samples.svc.cluster.local -uroot -p
```
