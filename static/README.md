# static

`static` 名前空間を作成。

```
$ kubectl apply -f ./namespace.yaml
namespace/static created
```

`traefik` をインストール。

```
$ helm repo add traefik https://traefik.github.io/charts
"traefik" has been added to your repositories

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

`traefik` の EXTERNAL-IP を確認（Vultr の Load Balancer の IP アドレス）。

```
$ kubectl -n static get service
NAME            TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)                      AGE
traefik         LoadBalancer   10.99.211.72    167.179.118.83   80:30770/TCP,443:32299/TCP   7m29s
```

`static.yantene.net` の A レコードをこれに向けておく。

`nginx` deployment と service を作成。

```
kubectl -n static apply -f ./nginx.yaml
kubectl -n static apply -f ./nginx-service.yaml
```

`ingress-traefik` ingress を作成。

```
kubectl -n static apply -f ./ingress-traefik.yaml
```

## 参考リンク

- https://doc.traefik.io/traefik/providers/kubernetes-ingress/#enabling-and-using-the-provider
- https://artifacthub.io/packages/helm/bitnami/nginx
