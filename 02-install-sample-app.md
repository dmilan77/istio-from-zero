## install the app

```
kubectl create ns book-info
kubectl -n book-info apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)
```
# kubectl label namespace book-info istio-injection=enabled

## verify the app
```
kubectl -n book-info exec "$(kubectl -n book-info get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl productpage:9080/produ
ctpage
```

## Install ingress gateway
```
kubectl -n book-info apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl -n book-info  get gateway

```

## Get Ingressgateway HOST
```
kubectl  -n istio-system get svc istio-ingressgateway
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
export TCP_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="tcp")].port}')
echo http://${INGRESS_HOST}:${INGRESS_PORT}/productpage

```