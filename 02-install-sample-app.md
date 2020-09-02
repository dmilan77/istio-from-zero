## install the app
- Deploy sample application
- Dashboards Tour
- Configure DNS

```
export NS=book-info
kubectl create ns ${NS}
kubectl -n ${NS} apply -f <(istioctl kube-inject -f samples/bookinfo/platform/kube/bookinfo.yaml)

```
# kubectl label namespace ${NS} istio-injection=enabled

## verify the app
```
kubectl -n ${NS} exec "$(kubectl -n ${NS} get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl productpage:9080/productpage

```

## Install ingress gateway
```
kubectl -n ${NS} apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
kubectl -n ${NS}  get gateway

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