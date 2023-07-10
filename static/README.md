# static

## リソースの作成

以下のリソースを作成する。

- Namespace `static`
- Deployment `nginx`
- Service `nginx-service`
- Ingress `ingress-traefik`

```
$ kubectl apply -f ./static.yaml
```

## Traefik のインストール

リポジトリの追加。

```
$ helm repo add traefik https://traefik.github.io/charts
"traefik" has been added to your repositories
```

Traefik のインストール。

```
$ helm install -n static traefik traefik/traefik
NAME: traefik
LAST DEPLOYED: Sat Jul  1 18:00:56 2023
NAMESPACE: static
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Traefik Proxy v2.10.1 has been deployed successfully on static namespace !
```

## DNS レコードの設定

`traefik` の EXTERNAL-IP を確認（Vultr の Load Balancer の IP アドレス）。

```
$ kubectl -n static get service
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
nginx-service   ClusterIP      10.111.106.211   <none>           80/TCP                       5m12s
traefik         LoadBalancer   10.103.206.83    167.179.111.41   80:31997/TCP,443:31489/TCP   4m46s
```

`static.yantene.net` の A レコードをこれに向けておく。

## 参考リンク

- https://doc.traefik.io/traefik/providers/kubernetes-ingress/#enabling-and-using-the-provider
- https://artifacthub.io/packages/helm/bitnami/nginx
