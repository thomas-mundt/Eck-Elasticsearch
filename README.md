# Eck-Elasticsearch

## Links
- https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-deploy-elasticsearch.html

## Setup

```
kubectl create -f https://download.elastic.co/downloads/eck/2.7.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.7.0/operator.yaml
```

## Create Elastic

```
cat <<EOF | kubectl apply -f -
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: quickstart
spec:
  version: 8.7.0
  nodeSets:
  - name: default
    count: 1
    config:
      node.store.allow_mmap: false
EOF
```

```
cat <<EOF | kubectl apply -f -
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: quickstart
spec:
  version: 8.7.0
  nodeSets:
  - name: default
    count: 1
    config:
      node.store.allow_mmap: false
      node.master: true
      node.data: true
      node.ingress: true
    podTemplate:
      spec:
        containers:
        - name: elasticsearch
          resources:
            limits:
              memory: 1Gi
              cpu: 900m
EOF
```


```
apiVersion: elasticsearch.k8s.elastic.co/v1
kind: Elasticsearch
metadata:
  name: es-cluster
  namespace: monitoring
spec:
  version: 8.7.0
  image: docker.elastic.co/elasticsearch/elasticsearch:8.7.0
  nodeSets:
  - name: default
    count: 1
    config:
      node.store.allow_mmap: false
```


## commands

```
kubectl get elasticsearch

```


## Deploy Kibana


```
cat <<EOF | kubectl apply -f -
apiVersion: kibana.k8s.elastic.co/v1
kind: Kibana
metadata:
  name: quickstart
spec:
  version: 8.7.0
  count: 1
  elasticsearchRef:
    name: quickstart
EOF
```

```
kubectl get kibana
```

```
kubectl get secret quickstart-es-elastic-user -o go-template='{{.data.elastic | base64decode}}'

```
