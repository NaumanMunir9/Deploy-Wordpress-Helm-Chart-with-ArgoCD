# Deploy Wordpress Helm Chart with ArgoCD

## ArgoCD Architecture

![ArgoCD Architecture](https://argo-cd.readthedocs.io/en/stable/assets/argocd_architecture.png)

---

### Setup Local Kubernetes Environment (KinD) with LoadBalancer (Metallb)

Please refer to the following github repo for setting up a local kubernetes environment using KinD and LoadBalancer using Metallb.

[Create Multi-Node Local Kubernetes Cluster (KinD) with LoadBalancer (Metallb)](https://github.com/NaumanMunir9/Create-Multi-Node-Local-Kubernetes-Cluster--KinD--with-LoadBalancer--Metallb-)

---

## Project WorkFlow

### Installing ArgoCD

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

```shell
k get all -n argocd
```

---

### Change the service type of "argocd-server" from "ClusterIP" to LoadBalancer

For the argocd to utilize Metallb, we have to change the service type of "argocd-server" service from "ClusterIP" to "LoadBalancer"

```shell
k edit service argocd-server -n argocd
```

```shell
k get all -n argocd
```

**Now we see that the service type of "argocd-server" service has been changed from "ClusterIP" to "LoadBalancer".**

---

### Login ArgoCD Web Interface

username: admin
password: "**please enter the following command in the terminal**"

```shell
k -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

---

### Create Application

- ***SYNC POLICY***: Automatic
- **PRUNE RESOURCES**: CHECKED, Argo will delete resources if they are no longer defined in Git
- **SELF HEAL**: CHECKED, Argo will force the state defined in Git into the cluster when a deviation in the cluster is detected

***DESTINATION***

- **Cluster URL**: https://kubernetes.default.svc
- **Namespace**: default

***DIRECTORY***

- **DIRECTORY RECURSE**: CHECKED

---
