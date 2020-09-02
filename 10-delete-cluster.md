```
export CLUSTER_NAME=kubeatlgke
export CLUSTER_ZONE=us-central1-b
export CLUSTER_VERSION=latest
gcloud beta container clusters delete $CLUSTER_NAME --zone=${CLUSTER_ZONE} --quiet
rm -rf ~/.kube

```