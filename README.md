------------------ urls -----------------------

for grafana: 192.168.1.208:3000

------------------  adding helm charts repo ----------------
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

--------------- lgtm stack installation ----------------
kubectl create namespace lgtm
helm upgrade --install prometheus prometheus-community/prometheus -n lgtm -f prometheus-values.yaml
helm upgrade --install loki grafana/loki -n lgtm -f loki-values.yaml
helm upgrade --install tempo grafana/tempo -n lgtm -f tempo-values.yaml
helm upgrade --install grafana grafana/grafana -n lgtm -f grafana-values.yaml

-----------------  port forwarding for grafana ------------
kubectl port-forward --address 192.168.1.208 svc/grafana 3000:3000 -n lgtm

----------------- install Alloy and Beyla in monitoring namespace -----------------
kubectl create namespace monitoring

kubectl create configmap alloy-config --namespace monitoring "--from-file=config.alloy=./alloy-config.alloy"
helm upgrade --install alloy grafana/alloy -n monitoring -f alloy-values.yaml

helm upgrade --install beyla grafana/beyla -n monitoring -f beyla-values.yaml


