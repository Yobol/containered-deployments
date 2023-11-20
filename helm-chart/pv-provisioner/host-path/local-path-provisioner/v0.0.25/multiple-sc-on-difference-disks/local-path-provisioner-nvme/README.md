# local-path-provisoner

只需要修改 `local-path-storage.yaml` 文件即可：

```YAML

apiVersion: apps/v1
kind: Deployment
metadata:
  name: local-path-provisioner-nvme
  namespace: local-path-storage-nvme
spec:
  replicas: 1
  selector:
    matchLabels:
      app: local-path-provisioner
  template:
    metadata:
      labels:
        app: local-path-provisioner
    spec:
      serviceAccountName: local-path-provisioner-service-account
      containers:
          ...
          env:
            ...
            - name: PROVISIONER_NAME
              value: rancher.io/local-path-nvme

kind: ConfigMap
apiVersion: v1
metadata:
  name: local-path-config
  namespace: local-path-storage-nvme
data:
  config.json: |-
    {
        "nodePathMap":[
            {
                "node":"DEFAULT_PATH_FOR_NON_LISTED_NODES",
                "paths":["/opt/local-path-provisioner"]
            }
        ]
    }
```

- 按 `-nvme` 修改相应配置；
- 给 `Deployment` 添加环境变量 `PROVISIONER_NAME: rancher.io/local-path-nvme`，并且修改 `StorageClass Provisioner` 的定义；
- 将 `ConfigMap` 中的 `/opt/local-path-provisioner` 修改为本地路径 `/nvme`；

```Shell
kustomize build | kubectl apply -f -
```

当前配置来源于 [rancher/local-path-provisioner](https://github.com/rancher/local-path-provisioner/tree/v0.0.25)。
