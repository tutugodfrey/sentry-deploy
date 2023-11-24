helm template sentry sentry/sentry --include-crds --output-dir sentry/base/
helm template sentry sentry/sentry --nohooks --include-crds --output-dir sentry2/base/


```bash
helm repo add sentry https://sentry-kubernetes.github.io/charts
helm install sentry sentry/sentry
helm install sentry sentry/sentry -f values.yaml
```


```bash
kubectl -n monitoring delete deployment my-kube-prometheus-stack-kube-state-metrics
kubectl -n monitoring delete deployment my-kube-prometheus-stack-operator
kubectl -n monitoring delete sts prometheus-my-kube-prometheus-stack-prometheus
kubectl -n monitoring delete daemonsets my-kube-prometheus-stack-prometheus-node-exporter
kubectl -n monitoring delete deployment my-kube-prometheus-stack-grafana
sleep 10
SERVICE=$(kubectl get svc -n monitoring | awk -F" " '{ if (NR!=1) print $1 }')
for S in $SERVICE; do kubectl -n monitoring delete svc $S; done;
```


```bash
git clone https://github.com/tutugodfrey/sentry-deploy

cd sentry-deploy/sentry/base/sentry/
kubectl apply -f templates/hooks/sentry-secret-create.yaml

kubectl apply -f templates/secret-snuba-env.yaml
kubectl apply -f templates/deployment-snuba-api.yaml
kubectl apply -f templates/service-snuba.yaml

kubectl apply -f templates/deployment-relay.yaml
kubectl apply -f templates/service-relay.yaml


cd sentry-deploy/sentry2/base/sentry/
kubectl apply -f charts/postgresql/templates/secrets.yaml
kubectl apply -f templates/configmap-sentry.yaml
kubectl apply -f templates/service-sentry.yaml
kubectl apply -f templates/pvc.yaml 
kubectl apply -f charts/redis/templates/
kubectl apply -f charts/redis/templates/master/
kubectl apply -f templates/deployment-sentry-web.yaml

kubectl apply -f templates/deployment-sentry-worker.yaml
kubectl apply -f templates/configmap-nginx.yaml
kubectl apply -f sentry2/base/sentry/charts/nginx/templates
kubectl apply -f templates/configmap-snuba.yaml
kubectl apply -f templates/deployment-snuba-api.yaml
kubectl apply -f templates/service-sentry.yaml
kubectl apply -f templates/service-snuba.yaml

kubectl apply -f charts/postgresql/templates/secrets.yaml
kubectl apply -f charts/postgresql/templates/primary/

kubectl apply -f charts/kafka/templates/
kubectl apply -f charts/kafka/charts/zookeeper/templates


kubectl apply -f templates/configmap-relay.yaml
kubectl apply -f templates/service-relay.yaml 

kubectl port-forward svc/sentry-web 30445:9000
kubectl port-forward svc/sentry-snuba 30446:1218
```