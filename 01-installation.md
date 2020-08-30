# Part 1: Introduce service mesh

- Provision a Kubernetes Cluster
- Service Mesh Overview
- Install Istio
- Deploy sample application
- Dashboards Tour
- Configure DNS

## Provision a Kubernetes Cluster
```
export CLUSTER_NAME=kubeatlgke
export CLUSTER_ZONE=us-central1-b
export CLUSTER_VERSION=latest

gcloud beta container clusters create $CLUSTER_NAME \
  --zone $CLUSTER_ZONE --num-nodes 3 \
  --machine-type "n1-standard-2" --image-type "COS" \
  --cluster-version=$CLUSTER_VERSION \
  --enable-stackdriver-kubernetes \
  --scopes "gke-default","compute-rw" \
  --enable-autoscaling --min-nodes 3 --max-nodes 8 \
  --enable-basic-auth

```

## Configure your kubeconfig
```
export GCLOUD_PROJECT=$(gcloud config get-value project)
gcloud container clusters get-credentials $CLUSTER_NAME \
  --zone $CLUSTER_ZONE --project $GCLOUD_PROJECT
# Grant admin permissions in the cluster to the current gcloud user:
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=$(gcloud config get-value core/account)
```

## Install ISTIO

```
export ISTIO_VERSION=1.7.0
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=$ISTIO_VERSION sh -
export PATH=$PWD/bin:$PATH


```

## Installation Configuration Profiles
|                          | default | demo | minimal | remote |
|--------------------------|---------|------|---------|--------|
| Core components          |         |      |         |        |
|     istio-egressgateway  |         | X    |         |        |
|     istio-ingressgateway | X       | X    |         |        |
|     istiod               | X       | X    | X       |        |


```
istioctl install --set profile=demo
```