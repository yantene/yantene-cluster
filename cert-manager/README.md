# cert-manager

## リソースの作成

以下のリソースを作成する。

- Namespace `cert-manager`

```
$ kubectl apply -f ./cert-manager.yaml
namespace/cert-manager created
```

## cert-manager のインストール

リポジトリの追加。

```
$ helm repo add jetstack https://charts.jetstack.io
"jetstack" has been added to your repositories
```

cert-manager のインストール。

```
$ helm install -n cert-manager cert-manager jetstack/cert-manager --set installCRDs=true
```

## ClusterIssuer の作成

```
$ kubectl apply -f ./lets-encrypt.yaml
```
