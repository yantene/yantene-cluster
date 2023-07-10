# Kubernetes Dashboard

## リソースの作成

以下のリソースを作成する。

- Namespace `kubernetes-dashboard`
- ServiceAccount `admin`
- ServiceAccount `admin` の `cluster-admin` への ClusterRoleBinding

```
$ kubectl apply -f ./kubernetes-dashboard/kubernetes-dashboard.yaml
namespace/kubernetes-dashboard created
serviceaccount/admin created
clusterrolebinding.rbac.authorization.k8s.io/binding-cluster-admin-admin created
```

## Kubernetes Dashboard のインストール

リポジトリの追加。

```
$ helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard
"kubernetes-dashboard" has been added to your repositories
```

Kubernetes Dashboard のインストール。

```
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

## Kubernetes Dashboard にログイン

Port forwarding する。

```
$ export POD_NAME=$(kubectl get pods -n kubernetes-dashboard -l "app.kubernetes.io/name=kubernetes-dashboard,app.kubernetes.io/instance=kubernetes-dashboard" -o jsonpath="{.items[0].metadata.name}")
$ kubectl -n kubernetes-dashboard port-forward $POD_NAME 8443:8443
```

Bearer Token を発行する。この Token は 1 時間の有効期限がある。

```
$ kubectl -n kubernetes-dashboard create token admin
...
```

ブラウザで [https://localhost:8443](https://localhost:8443) へアクセスし、Bearer Token を入力してログインする。

## 参考リンク

- https://kubernetes.io/ja/docs/tasks/access-application-cluster/web-ui-dashboard/
- https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md
