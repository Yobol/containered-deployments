# local-path-provisoner

只需要修改 `local-path-storage.yaml` 文件即可：

```YAML
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

将 `/opt/local-path-provisioner` 修改为本地路径。

```Shell
kustomize build | kubectl apply -f -
```

当前配置来源于 [rancher/local-path-provisioner](https://github.com/rancher/local-path-provisioner/tree/v0.0.25)。
