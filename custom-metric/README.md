# Custom metrics

Pre-requisite: Install prometheus and grafana (follow prometheus-grafana folder)

1. Run this custom application written in go (ref. https://prometheus.io/docs/tutorials/instrumenting_http_server_in_go/), which generates a metric named ping_request_count

go mod init prometheus_example

go mod tidy 

go run main.go  

Access  http://<ip>:8090/ping -> Response ""pong"
Watch the custom metric ping_request_count at http://<ip>:8090/metrics

2. Update prometheus config

Prometheus was installed uing configuration in the ConfigMap prometheus-config

Edit the ConfigMap from Dashboard, add following. Note: Used IP address of WSL2 below. or some reason it didnot work with localhost, even though go server was running in WSL2.  

  - job_name: sample_go_server
    static_configs:
      - targets: ["172.30.27.146:8090"]

3. Install prometheus-adapter ref. https://github.com/kubernetes-sigs/prometheus-adapter

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

helm install prometheus-adapter prometheus-community/prometheus-adapter --set config=prometheus-adapter-config.yaml

helm install prometheus-adapter prometheus-community/prometheus-adapter --set prometheus.url="http://172.30.27.146",prometheus.port="31000"  

Edit the ConfigMap prometheus-adapter



Now, list metrics using command: kubectl get --raw localhost:8001/apis/custom.metrics.k8s.io/v1beta1

kubectl get --raw "/apis/custom.metrics.k8s.io/v1beta1/namespaces/default/pods/*/ping_request_count_sd" 