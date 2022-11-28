# kubernetes-twemproxy

Running twemproxy on Kubernetes

## Install redis

Setup 2 instances of redis: create configmap for them, create 2 redis deployments and their respective services.

Update the PVC size to your liking.

```
kubectl create configmap redis-conf --from-file=redis.conf
kubectl create -f deployments/redis01.yaml
kubectl create -f deployments/redis02.yaml
kubectl create -f services/redis01.yaml
kubectl create -f services/redis02.yaml
```

## Install twemproxy

Build the docker image for twemproxy from `twemproxy` folder.

Edit twemproxy configuration

```
alpha:
  listen: 0.0.0.0:22121
  hash: murmur
  distribution: ketama
  auto_eject_hosts: true
  redis: true
  server_connections: 1
  server_retry_timeout: 30000
  timeout: 4000
  server_failure_limit: 3
  preconnect: true
  servers:
    - redis01:6379:1 redis01
    - redis02:6379:1 redis02
    # add more servers here
```

Create the configmap, deployment and service for twemproxy

```
kubectl create configmap twemproxy-conf --from-file=twemproxy.yaml
kubectl create -f deployments/twemproxy.yaml
kubectl create -f services/twemproxy.yaml
```

## Check the installation

Fire up an Ubuntu container and check if twemproxy is working as expected.

```
kubectl run -i --tty ubuntu --image=ubuntu --restart=Never /bin/bash
apt-get update && apt-get install -y redis-tools
redis-cli -h twemproxy -p 22121
set foo bar
get foo
```

