## Install prometheus, jaeger, grafana, kiali
```
kubectl -n istio-system apply -f samples/addons/jaeger.yaml 
kubectl -n istio-system apply -f samples/addons/kiali.yaml 
kubectl -n istio-system apply -f samples/addons/prometheus.yaml
kubectl -n istio-system apply -f samples/addons/grafana.yaml

```
## Open kiali dashboard
```
# bin/istioctl dashboard kiali
kubectl -n istio-system port-forward \
    $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001

```

## send some traffic

```
kubectl get svc istio-ingressgateway -n istio-system
export GATEWAY_URL=[EXTERNAL_URL]
# export GATEWAY_URL=34.121.35.192
watch -n 1 curl -o /dev/null -s -w %{http_code} $GATEWAY_URL/productpage


```