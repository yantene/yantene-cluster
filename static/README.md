# static

## リソースの作成

以下のリソースを作成する。

- Namespace `static`
- Deployment `nginx`
- Service `nginx-service`
- Ingress `ingress-traefik`
- Certificate `static-certificate`

```
$ kubectl apply -f ./static.yaml
```
