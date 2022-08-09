# ./kubernetes-manifests

# Run to deploy
```
kubectl apply -f kubenetes-manifests
```
# wavefront proxys + add otel proxy ports
```
helm install wavefront wavefront/wavefront \
    --set wavefront.url=https://XXXXX.wavefront.com \
    --set wavefront.token=XXXXXXXXXXXXXXXXXXXX \
    --set proxy.args="--otlpGrpcListenerPorts 4317 --otlpHttpListenerPorts 4318 --traceZipkinListenerPorts 9411" \
    --set clusterName="XXXXX-demo-otel" --namespace wavefront --create-namespace
```
# patch the wavefront proxy
```
kubectl -n wavefront patch svc wavefront-proxy --patch '{"spec": {"ports": [{"name":"oltphttp", "port": 4318, "protocol": "TCP"}, {"name":"oltpgrpc", "port": 4317, "protocol": "TCP"}]}}'
```
