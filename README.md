[TOC]

# dragonfly devops

## 简介

本项目是 dragonfly 的部署工具，用于快速部署 dragonfly 项目。

## 依赖
- helmfile: https://github.com/helmfile/helmfile
- helm: https://helm.sh/
- helm-diff: https://github.com/databus23/helm-diff
- ceph csi: rook

## mysql 初始化
```shell
kubectl port-forward -n mysql-innodbcluster-d7y service/mysql-innodbcluster-d7y  3306


mysql -h127.0.0.1 -udragonfly -p
CREATE DATABASE manager CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

## dragonfly 部署
### 需要具体查看environments/dev/中对应的配置是否符合要求，然后执行以下命令

1. mysql-operator 部署
```shell
# environments/dev/base.yaml.gotmpl
install:
  mysql_operator: true 
helmfile -f helmfile.yaml -e dev sync
```
2. mysql-innodbcluster 部署
```shell
# environments/dev/base.yaml.gotmpl
install:
  mysql_innodbcluster_d7y: true
helmfile -f helmfile.yaml -e dev sync
# mysql-innodbcluster-d7y
kubectl port-forward -n mysql-innodbcluster-d7y service/mysql-innodbcluster-d7y  3306
mysql -h127.0.0.1 -udragonfly -p
CREATE DATABASE manager CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
3. redis 部署
```shell
# environments/dev/base.yaml.gotmpl
install:
  d7y_redis_sentinel: true
helmfile -f helmfile.yaml -e dev sync
```

4. dragonfly 部署
```shell
# environments/dev/base.yaml.gotmpl
install:
  d7y_system: true
helmfile -f helmfile.yaml -e dev sync
```