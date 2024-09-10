#deploy opensearch
helm repo add opensearch https://opensearch-project.github.io/helm-charts/
helm repo update
helm upgrade --install opensearch  \
opensearch/opensearch -f opensearch-values.yaml

---
#deploy Opensearch dashboard
helm repo add opensearch https://opensearch-project.github.io/helm-charts/
helm repo update

helm upgrade --install opensearch-dashboards \
opensearch/opensearch-dashboards -f ops-dashboard.yaml

---
#access dashboard
kubectl port-forward svc/opensearch-dashboards 5601:5601


---
#create
kubectl apply -f data-prepper-config.yaml
kubectl apply -f data-prepper-deploy.yaml

---
#Deploy cert-manager
```
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm upgrade --install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.12.0 \
  --set installCRDs=true

---
#deploy opentelemetry-operator
kubectl apply -f https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml 
```

---
#deploy OpenTelemetryCollector
```
kubectl apply -f OpenTelemetryCollector.yaml

```


---
#deploy Instrumentation
```
kubectl create namespace petstore
kubectl apply -n petstore -f Instrumentation.yaml
```

---
#deploy simple app
```
kubectl create ns petstore
kubectl apply -n petstore -f spring-petclinic.yaml

```
---
#inject-java 
```
kubectl patch -n petstore deployment.apps/spring-petclinic \
 -p '{"spec": {"template": {"metadata": {"annotations": {"instrumentation.opentelemetry.io/inject-java": "true"}}}}}'
```