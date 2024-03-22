# k8s-airflow-minio-spark

### Pré-requisitos
- Instalar o [docker](https://docs.docker.com/)
- Instalar kubectl e k8s [local](https://kubernetes.io/docs/tasks/tools/)
- Instalar o [Helm](https://helm.sh/docs/intro/install/)
- Instalar o [Lens](https://k8slens.dev/)

Opcional: https://github.com/ahmetb/kubectx

## Deploy dashboard UI for Kubernetes

### Step 1: Deploying the dashboard UI
https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```
  kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml
```

### Step 2: Create Admin User and Get Bearer Token

#### Creating a Service Account
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md

#### Deploy the admin user role
```
  kubectl apply -f dashboard-admin.yaml
```

#### Get the Bearer Token for the user
```
  kubectl -n kubernetes-dashboard create token admin-user
```
```
  kubectl create clusterrolebinding permissive-binding --clusterrole=admin-user --user=admin --user=kubelet --group=system:serviceaccounts:ci
```

### Step 3: Enable the Dashboard
```
  kubectl proxy
```
Dashboard: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/.

### Step 4: Login



