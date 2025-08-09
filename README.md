# Grafana LGTM stack installation using Helm chart
## Grafana Stack
![](https://github.com/bahmanzadeh/lgtm/blob/main/lgtm-stack.png)
## Grafana Architecture with OpenTelemetry Integration
![](https://github.com/bahmanzadeh/lgtm/blob/main/lgtm.png)
----
## Adding helm charts repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```
## lgtm stack installation
```bash
kubectl create namespace lgtm
helm upgrade --install prometheus prometheus-community/prometheus -n lgtm -f prometheus-values.yaml
helm upgrade --install loki grafana/loki -n lgtm -f loki-values.yaml
helm upgrade --install tempo grafana/tempo -n lgtm -f tempo-values.yaml
helm upgrade --install grafana grafana/grafana -n lgtm -f grafana-values.yaml
```
## Port forwarding for Grafana
```bash
kubectl port-forward --address 192.168.1.208 svc/grafana 3000:3000 -n lgtm
```
Connect to Grafana: http://192.168.1.208:3000
----
## Bonus: Install Alloy and Beyla in the monitoring namespace
```bash
kubectl create namespace monitoring
kubectl create configmap alloy-config --namespace monitoring "--from-file=config.alloy=./alloy-config.alloy"
helm upgrade --install alloy grafana/alloy -n monitoring -f alloy-values.yaml
helm upgrade --install beyla grafana/beyla -n monitoring -f beyla-values.yaml
```
