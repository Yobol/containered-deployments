# Kubeflow

>  https://github.com/kubeflow/manifests/tree/v1.8.0

## 安装

```shell
wget https://github.com/kubeflow/manifests/archive/refs/tags/v1.8.0.tar.gz -O manifests-1.8.0.tar.gz
tar -xf manifests-1.8.0.tar.gz
cd manifests-1.8.0

while ! kustomize build example | kubectl apply -f -; do echo "Retrying to apply resources"; sleep 10; done
```
