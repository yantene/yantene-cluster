# traefik

## リソースの作成

以下のリソースを作成する。

- Namespace `traefik`

```
$ kubectl apply -f ./traefik.yaml
```

## Traefik のインストール

リポジトリの追加。

```
$ helm repo add traefik https://traefik.github.io/charts
"traefik" has been added to your repositories
```

Traefik のインストール。

```
$ helm install -n traefik traefik traefik/traefik
NAME: traefik
LAST DEPLOYED: Sat Jul  1 18:00:56 2023
NAMESPACE: traefik
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Traefik Proxy v2.10.1 has been deployed successfully on traefik namespace !
```

## DNS レコードの設定

`traefik` の EXTERNAL-IP を確認（Vultr の Load Balancer の IP アドレス）。

```
$ kubectl -n traefik get service
NAME      TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                      AGE
traefik   LoadBalancer   10.105.129.20   167.179.111.97   80:32577/TCP,443:30108/TCP   72m
```

`static.yantene.net` の A レコードをこれに向けておく。

## 参考リンク

- https://doc.traefik.io/traefik/providers/kubernetes-ingress/#enabling-and-using-the-provider
- https://artifacthub.io/packages/helm/bitnami/nginx
