# Kubernetes Dashboard

`kubernetes-dashboard` 名前空間を作成。

```
$ kubectl apply -f ./namespace.yaml
namespace/kubernetes-dashboard created
```

`kubernetes-dashboard` をインストール。

```
$ helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard
"kubernetes-dashboard" has been added to your repositories

$ helm install -n kubernetes-dashboard kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard
NAME: kubernetes-dashboard
LAST DEPLOYED: Sat Jul  1 16:22:15 2023
NAMESPACE: kubernetes-dashboard
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
*********************************************************************************
*** PLEASE BE PATIENT: kubernetes-dashboard may take a few minutes to install ***
*********************************************************************************

Get the Kubernetes Dashboard URL by running:
  export POD_NAME=$(kubectl get pods -n kubernetes-dashboard -l "app.kubernetes.io/name=kubernetes-dashboard,app.kubernetes.io/instance=kubernetes-dashboard" -o jsonpath="{.items[0].metadata.name}")
  echo https://127.0.0.1:8443/
  kubectl -n kubernetes-dashboard port-forward $POD_NAME 8443:8443
```

サービスアカウント `admin` を作成。

```
$ kubectl apply -f ./create-admin.yaml
serviceaccount/admin created
```

サービスアカウント `admin` を `cluster-admin` に紐付け。
これは名前空間内に作られないことに注意。

```
$ kubectl apply -f ./cluster-role-binding-cluster-admin-admin.yaml
clusterrolebinding.rbac.authorization.k8s.io/binding-cluster-admin-admin created
```

Bearer Token を発行。

```
$ kubectl -n kubernetes-dashboard create token admin-user
...
```

Port forwarding してログイン。

```
$ export POD_NAME=$(kubectl get pods -n kubernetes-dashboard -l "app.kubernetes.io/name=kubernetes-dashboard,app.kubernetes.io/instance=kubernetes-dashboard" -o jsonpath="{.items[0].metadata.name}")
  echo https://127.0.0.1:8443/
  kubectl -n kubernetes-dashboard port-forward $POD_NAME 8443:8443
```

## 参考リンク

- https://kubernetes.io/ja/docs/tasks/access-application-cluster/web-ui-dashboard/
- https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md
