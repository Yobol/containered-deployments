# Ingress-NGINX

> https://github.com/kubernetes/ingress-nginx

## 安装

```shell
helm show values ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --version v4.8.4 > default.yaml
vim values.yaml

helm upgrade --install ingress-nginx \
  ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --version v4.8.4 \
  --namespace ingress-nginx --create-namespace \
  -f values.yaml
```

当网络不好时，可以直接下载 `tgz` 包到本地进行安装：

```shell
wget https://github.com/kubernetes/ingress-nginx/releases/download/helm-chart-4.8.4/ingress-nginx-4.8.4.tgz

helm show values ingress-nginx-4.8.4.tgz > default.yaml
vim values.yaml

helm upgrade --install ingress-nginx \
  ingress-nginx-4.8.4.tgz \
  --namespace ingress-nginx --create-namespace \
  -f default.yaml -f values.yaml
```

> values.yaml

```yaml
controller:
  image:
    registry: k8s.dockerproxy.com
  service:
    externalIPs:
    - <Host External IP>
```

## 验证

### 本地验证

> https://kubernetes.github.io/ingress-nginx/deploy/#local-testing

```shell
# create a simple web server and the associated service
kubectl create deployment demo --image=httpd --port=80
kubectl expose deployment demo

# create an ingress resource
kubectl create ingress demo-localhost --class=nginx \
  --rule="demo.localdev.me/*=demo:80"

# visit the deployment using curl
curl --resolve demo.localdev.me:8080:127.0.0.1 http://demo.localdev.me:8080
```
