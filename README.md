# Dragonfly DevOps

## Introduction
This project is a deployment tool for Dragonfly, used to quickly deploy Dragonfly projects.

## Dependencies
- helmfile: https://github.com/helmfile/helmfile
- helm: https://helm.sh/
- helm-diff: https://github.com/databus23/helm-diff
- csi

## Dragonfly Deployment

### Ensure the configuration in environments/dev/ meets the requirements, then execute the following commands
* d7y-mysql.yaml.gotmpl: mysql InnoDB configuration 
* d7y-redis-sentinel. yaml.gotmpl: redis sentinel configuration
* d7y-system.yaml.gotmpl: dragonfly system configuration

1. MySQL Operator Deployment
```shell
# environments/dev/base.yaml.gotmpl
install:
  mysql_operator: true
helmfile -f helmfile.yaml -e dev sync
```

2. MySQL InnoDB Cluster Deployment
```shell
# environments/dev/base.yaml.gotmpl
install:
  mysql_innodbcluster_d7y: true
helmfile -f helmfile.yaml -e dev sync

# mysql-innodbcluster-d7y
kubectl port-forward -n mysql-innodbcluster-d7y service/mysql-innodbcluster-d7y 3306
mysql -h127.0.0.1 -udragonfly -p
CREATE DATABASE manager CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

3. Redis Sentinel Deployments
```shell
# environments/dev/base.yaml.gotmpl
install:
  d7y_redis_sentinel: true
helmfile -f helmfile.yaml -e dev sync
```

4. Dragonfly Deployment
```shell
# environments/dev/base.yaml.gotmpl
install:
  d7y_system: true
helmfile -f helmfile.yaml -e dev sync
```

5. Dragonfly update 
```shell
# diff will show the difference between the current state and the desired state 
helmfile -f helmfile.yaml -e dev diff

# sync will update the current state to the desired state
helmfile -f helmfile.yaml -e dev sync
```