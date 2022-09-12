
- [Envoy introduction](#envoy-introduction)
  - [create envoy config](#create-envoy-config)
  - [Change call timeout](#change-call-timeout)
  - [Retrying failed requests](#retrying-failed-requests)
- [Installing Istio](#installing-istio)
  - [Istio CLI installation](#istio-cli-installation)
  - [Installing Istio](#installing-istio-1)
  - [Install sidecar for demo app](#install-sidecar-for-demo-app)
- [Connecting To Observability Systems](#connecting-to-observability-systems)
  - [Add Istio Dashboards to Grafana](#add-istio-dashboards-to-grafana)
  - [Scraping the Istio service mesh: control plane](#scraping-the-istio-service-mesh-control-plane)
  - [Scraping the Istio service mesh: data plane](#scraping-the-istio-service-mesh-data-plane)
  - [Installing Kiali](#installing-kiali)
- [Creating An Ingress Gateway For Istio](#creating-an-ingress-gateway-for-istio)
  - [Install Istio ingress gateway](#install-istio-ingress-gateway)

## Envoy introduction

```shell
# cat labs/01/httpbin.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: httpbin
---
apiVersion: v1
kind: Service
metadata:
  name: httpbin
  labels:
    app: httpbin
    service: httpbin
spec:
  ports:
  - name: http
    port: 8000
    targetPort: 80
  selector:
    app: httpbin
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpbin
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpbin
      version: v1
  template:
    metadata:
      labels:
        app: httpbin
        version: v1
    spec:
      serviceAccountName: httpbin
      containers:
      - image: docker.io/kennethreitz/httpbin
        imagePullPolicy: IfNotPresent
        name: httpbin
        ports:
        - containerPort: 80

```

```shell

# cat labs/01/sleep.yaml

# Copyright Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

##################################################################################################
# Sleep service
##################################################################################################
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sleep
---
apiVersion: v1
kind: Service
metadata:
  name: sleep
  labels:
    app: sleep
    service: sleep
spec:
  ports:
  - port: 80
    name: http
  selector:
    app: sleep
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sleep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sleep
  template:
    metadata:
      labels:
        app: sleep
    spec:
      serviceAccountName: sleep
      containers:
      - name: sleep
        image: governmentpaas/curl-ssl:terraform-14
        command: ["/bin/sleep", "3650d"]
        imagePullPolicy: IfNotPresent
        volumeMounts:
        - mountPath: /etc/sleep/tls
          name: secret-volume
      volumes:
      - name: secret-volume
        secret:
          secretName: sleep-secret
          optional: true
---
```

```shell
kubectl apply -f labs/01/httpbin.yaml
kubectl apply -f labs/01/sleep.yaml
```

Verify access from sleep pod 

```shell
kubectl exec deploy/sleep -- curl httpbin:8000/headers
root@kubernetes:~/istio-workshops/deploy-istio# kubectl exec deploy/sleep -- curl httpbin:8000/headers
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   106  100   106    0     0  15142      0 --:--:-- --:--:-- --:--:-- 15142
{
  "headers": {
    "Accept": "*/*",
    "Host": "httpbin:8000",
    "User-Agent": "curl/7.69.1"
  }
}
```
### create envoy config

```yaml
admin:
  accessLogPath: /dev/stdout
  address:
    socketAddress:
      address: 0.0.0.0
      portValue: 15000
staticResources:
  listeners:
  - name: httpbin-listener
    address:
      socketAddress:
        address: 0.0.0.0
        portValue: 15001
    filterChains:
    - filters:
      - name: envoy.filters.network.http_connection_manager
        typedConfig:
          '@type': type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
          httpFilters:
          - name: envoy.filters.http.router
          routeConfig:
            name: simple_httpbin_route
            virtualHosts:
            - domains:
              - '*'
              name: httpbin_host
              routes:
              - match:
                  prefix: /
                route:
                  cluster: httpbin_service
          statPrefix: httpbin
  clusters:
  - connectTimeout: 5s
    loadAssignment:
      clusterName: httpbin_service
      endpoints:
      - lbEndpoints:
        - endpoint:
            address:
              socketAddress:
                address: httpbin
                portValue: 8000
    name: httpbin_service
    respectDnsTtl: true
    dnsLookupFamily: V4_ONLY
    type: STRICT_DNS
    upstreamConnectionOptions:
      tcpKeepalive: {}
```

deoply
```shell
kubectl create cm envoy --from-file=envoy.yaml=./labs/01/envoy-conf.yaml -o yaml --dry-run=client | kubectl apply -f -
kubectl apply -f labs/01/envoy-proxy.yaml
```

Verify(note enriched response headers, `X-Envoy-Expected-Rq-Timeout-Ms`)

```shell
root@kubernetes:~/istio-workshops/deploy-istio# kubectl exec deploy/sleep -- curl http://envoy/headers
{
  "headers": {
    "Accept": "*/*",
    "Content-Length": "0",
    "Host": "envoy",
    "User-Agent": "curl/7.69.1",
    "X-Envoy-Expected-Rq-Timeout-Ms": "15000"
  }
}
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   175  100   175    0     0  35000      0 --:--:-- --:--:-- --:--:-- 43750
```

### Change call timeout

```yaml
routeConfig:
  name: simple_httpbin_route
  virtualHosts:
  - domains:
    - '*'
    name: httpbin_host
    routes:
    - match:
        prefix: /
      route:
        cluster: httpbin_service
        timeout: 1s
```

```shell
kubectl create cm envoy --from-file=envoy.yaml=./labs/01/envoy-conf-timeout.yaml -o yaml --dry-run=client | kubectl apply -f -

## restart envoy
kubectl rollout restart deploy/envoy
kubectl exec deploy/sleep -- curl http://envoy/headers
```

Test it

```shell
kubectl exec deploy/sleep -- curl -v http://envoy/delay/5
```

Envoy exposes a /stats endpoint we can inspect:

```shell
kubectl exec deploy/sleep -- curl http://envoy:15000/stats
kubectl exec deploy/sleep -- curl http://envoy:15000/stats | grep retry
```

### Retrying failed requests

```yaml
routeConfig:
  name: simple_httpbin_route
  virtualHosts:
  - domains:
    - '*'
    name: httpbin_host
    routes:
    - match:
        prefix: /
      route:
        cluster: httpbin_service
        timeout: 1s
        retryPolicy:
          retryOn: 5xx
          numRetries: 3
```          

```shell
kubectl create cm envoy --from-file=envoy.yaml=./labs/01/envoy-conf-retry.yaml -o yaml --dry-run=client | kubectl apply -f -
kubectl rollout restart deploy/envoy

kubectl exec deploy/sleep -- curl -v http://envoy/status/500

kubectl exec deploy/sleep -- curl http://envoy:15000/stats | grep retry
```

## Installing Istio

Create namespace and services

```shell
kubectl create ns istioinaction
```

```shell
# cat sample-apps/web-api.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: web-api
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-api
  labels:
    app: web-api
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web-api
      version: v1
  template:
    metadata:
      labels:
        app: web-api
        version: v1
      annotations:
    spec:
      serviceAccountName: web-api
      containers:
      - name: web-api
        image: nicholasjackson/fake-service:v0.7.8
        ports:
        - containerPort: 8080
        env:
        - name: "LISTEN_ADDR"
          value: "0.0.0.0:8080"
        - name: "UPSTREAM_URIS"
          value: "http://recommendation:8080"
        - name: "NAME"
          value: "web-api"
        - name: "MESSAGE"
          value: "Hello From Web API"

---
# Service to expose web frontend

apiVersion: v1
kind: Service
metadata:
  name: web-api
spec:
  selector:
    app: web-api
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 8080
```

```shell 
# cat sample-apps/recommendation.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: recommendation
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recommendation
  labels:
    app: recommendation
spec:
  replicas: 1
  selector:
    matchLabels:
      app: recommendation
      version: v1
  template:
    metadata:
      labels:
        app: recommendation
        version: v1
      annotations:
    spec:
      serviceAccountName: recommendation
      containers:
      - name: recommendation
        image: nicholasjackson/fake-service:v0.7.8
        ports:
        - containerPort: 8080
        env:
        - name: "LISTEN_ADDR"
          value: "0.0.0.0:8080"
        - name: "UPSTREAM_URIS"
          value: "http://purchase-history:8080"
        - name: "NAME"
          value: "recommendation"
        - name: "MESSAGE"
          value: "Hello From Recommendations!"

---
# Service to expose recommendation frontend

apiVersion: v1
kind: Service
metadata:
  name: recommendation
spec:
  selector:
    app: recommendation
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 8080
```

```shell
# cat sample-apps/purchase-history-v1.yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: purchase-history
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: purchase-history-v1
  labels:
    app: purchase-history-v1
spec:
  replicas: 1
  selector:
    matchLabels:
        app: purchase-history
        version: v1
  template:
    metadata:
      labels:
        app: purchase-history
        version: v1
      annotations:
    spec:
      serviceAccountName: purchase-history
      containers:
      - name: purchase-history
        image: nicholasjackson/fake-service:v0.7.8
        ports:
        - containerPort: 8080
        env:
        - name: "LISTEN_ADDR"
          value: "0.0.0.0:8080"
        - name: "NAME"
          value: "purchase-history-v1"
        - name: "SERVER_TYPE"
          value: "http"
        - name: "MESSAGE"
          value: "Hello From Purchase History (v1)!"

---
# Service to expose purchase-history-v1 frontend

apiVersion: v1
kind: Service
metadata:
  name: purchase-history
spec:
  selector:
    app: purchase-history
  ports:
  - name: http
    protocol: TCP
    port: 8080
    targetPort: 8080
```

```shell
# cat sample-apps/sleep.yaml
# Copyright 2017 Istio Authors
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

##################################################################################################
# Sleep service
##################################################################################################
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sleep
---
apiVersion: v1
kind: Service
metadata:
  name: sleep
  labels:
    app: sleep
spec:
  ports:
  - port: 80
    name: http
  selector:
    app: sleep
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sleep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sleep
  template:
    metadata:
      labels:
        app: sleep
    spec:
      serviceAccountName: sleep
      containers:
      - name: sleep
        image: governmentpaas/curl-ssl:terraform-14
        command: ["/bin/sleep", "3650d"]
        imagePullPolicy: IfNotPresent
        volumeMounts:
        - mountPath: /etc/sleep/tls
          name: secret-volume
      volumes:
      - name: secret-volume
        secret:
          secretName: sleep-secret
          optional: true
---
```

```shell
kubectl apply -n istioinaction -f sample-apps/web-api.yaml
kubectl apply -n istioinaction -f sample-apps/recommendation.yaml
kubectl apply -n istioinaction -f sample-apps/purchase-history-v1.yaml
kubectl apply -n istioinaction -f sample-apps/sleep.yaml

## check pods
kubectl get po -n istioinaction
```

### Istio CLI installation

```
$ curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.8.3 sh -
$ export PATH=$PWD/istio-1.8.3/bin:$PATH
$ istioctl version
no running Istio pods in "istio-system"
1.8.3
```

Three ways to install IstioT

- istioctl CLI tool
- Istio Operator
- Helm

### Installing Istio

```shell
kubectl create ns istio-system
```

```shell
# cat labs/02/istiod-service.yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: istiod
    istio: pilot
    release: istio
  name: istiod
  namespace: istio-system
spec:
  type: ClusterIP
  ports:
  - name: grpc-xds
    port: 15010
  - name: https-dns
    port: 15012
  - name: https-webhook
    port: 443
    targetPort: 15017
  - name: http-monitoring
    port: 15014
  selector:
    app: istiod

```


```shell
kubectl apply -f labs/02/istiod-service.yaml
```

Sample istio operator:

```yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  name: control-plane
spec:
  profile: minimal
```

```
$ istioctl install -y -n istio-system -f labs/02/control-plane.yaml --revision 1-8-3
✔ Istio core installed
✔ Istiod installed
✔ Installation complete
```

```shell
kubectl get pod -n istio-system

# query the Istio control plane's debug endpoints
kubectl exec -n istio-system deploy/istiod-1-8-3 -- pilot-discovery request GET /debug/registryz
```

### Install sidecar for demo app

```shell
istioctl kube-inject -f labs/01/httpbin.yaml --meshConfigMapName istio-1-8-3 --injectConfigMapName istio-sidecar-injector-1-8-3  | kubectl apply -f -
```

## Connecting To Observability Systems

```shell
kubectl create ns prometheus

## add helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Install prometheus with custom values

```shell
helm install prom prometheus-community/kube-prometheus-stack --version 13.13.1 -n prometheus -f labs/03/prom-values.yaml

kubectl get po -n prometheus
```

Open port forward to test Promethues

```shell
kubectl -n prometheus port-forward statefulset/prometheus-prom-kube-prometheus-stack-prometheus 9090 --address 0.0.0.0
```
Grafana

```shell
kubectl -n prometheus port-forward svc/prom-grafana 3000:80 --address 0.0.0.0
```

### Add Istio Dashboards to Grafana

```shell
kubectl -n prometheus create cm istio-dashboards \
--from-file=pilot-dashboard.json=labs/03/dashboards/pilot-dashboard.json \
--from-file=istio-workload-dashboard.json=labs/03/dashboards/istio-workload-dashboard.json \
--from-file=istio-service-dashboard.json=labs/03/dashboards/istio-service-dashboard.json \
--from-file=istio-performance-dashboard.json=labs/03/dashboards/istio-performance-dashboard.json \
--from-file=istio-mesh-dashboard.json=labs/03/dashboards/istio-mesh-dashboard.json \
--from-file=istio-extension-dashboard.json=labs/03/dashboards/istio-extension-dashboard.json
```

Label the configmap

```shell
kubectl label -n prometheus cm istio-dashboards grafana_dashboard=1
```

Port forward again and check Grafana

```shell
kubectl -n prometheus port-forward svc/prom-grafana 3000:80 --address 0.0.0.0
```

### Scraping the Istio service mesh: control plane

set up a ServiceMonitor to scrape the Istio control-plane components:

```shell
# cat labs/03/monitor-control-plane.yaml
---
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: istio-component-monitor
  namespace: prometheus
  labels:
    monitoring: istio-components
    release: prom
spec:
  jobLabel: istio
  targetLabels: [app]
  selector:
    matchExpressions:
    - {key: istio, operator: In, values: [pilot]}
  namespaceSelector:
    any: true
  endpoints:
  - port: http-monitoring
    interval: 15s
```

Apply

```shell
kubectl apply -f labs/03/monitor-control-plane.yaml
```

### Scraping the Istio service mesh: data plane

```shell
# cat labs/03/monitor-data-plane.yaml

apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: envoy-stats-monitor
  namespace: prometheus
  labels:
    monitoring: istio-proxies
    release: prom
spec:
  selector:
    matchExpressions:
    - {key: istio-prometheus-ignore, operator: DoesNotExist}
  namespaceSelector:
    any: true
  jobLabel: envoy-stats
  podMetricsEndpoints:
  - path: /stats/prometheus
    interval: 15s
    relabelings:
    - action: keep
      sourceLabels: [__meta_kubernetes_pod_container_name]
      regex: "istio-proxy"
    - action: keep
      sourceLabels: [__meta_kubernetes_pod_annotationpresent_prometheus_io_scrape]
    - sourceLabels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
      action: replace
      regex: ([^:]+)(?::\d+)?;(\d+)
      replacement: $1:$2
      targetLabel: __address__
    - action: labeldrop
      regex: "__meta_kubernetes_pod_label_(.+)"
    - sourceLabels: [__meta_kubernetes_namespace]
      action: replace
      targetLabel: namespace
    - sourceLabels: [__meta_kubernetes_pod_name]
      action: replace
      targetLabel: pod_name
```

Create it

```shell
kubectl apply -f labs/03/monitor-data-plane.yaml
```

generate some load to the data plane

```shell
for i in {1..10}; do kubectl exec deploy/sleep -n default -- curl http://httpbin.default:8000/headers; done
```

### Installing Kiali

Kiali is a networking graph dashboard that shows connectivity paths between services. 

```shell
kubectl create ns kiali-operator

helm install \
--set cr.create=true \
--set cr.namespace=istio-system \
--namespace kiali-operator \
--repo https://kiali.org/helm-charts \
--version 1.29.1 \
kiali-operator \
kiali-operator

kubectl get po -n kiali-operator
```

Create Kiali CR:

```shell
# cat labs/03/kiali.yaml
apiVersion: kiali.io/v1alpha1
kind: Kiali
metadata:
  namespace: istio-system
  name: kiali
spec:
  istio_namespace: "istio-system"
  istio_component_namespaces:
    prometheus: prometheus
  auth:
    strategy: token
  deployment:
    accessible_namespaces:
    - '**'
    image_version: operator_version
  external_services:
    prometheus:
      cache_duration: 10
      cache_enabled: true
      cache_expiration: 300
      url: "http://prom-kube-prometheus-stack-prometheus.prometheus:9090"
```

```shell
kubectl apply -f labs/03/kiali.yaml

kubectl get po -n istio-system

kubectl -n istio-system port-forward deploy/kiali 20001 --address 0.0.0.0
```

Create service account for Kiali

```shell
kubectl create serviceaccount kiali-dashboard -n istio-system
kubectl create clusterrolebinding kiali-dashboard-admin --clusterrole=cluster-admin --serviceaccount=istio-system:kiali-dashboard

## get token
kubectl get secret -n istio-system -o jsonpath="{.data.token}" $(kubectl get secret -n istio-system | grep kiali-dashboard | awk '{print $1}' ) | base64 --decode
```

you can switch to anonymous login with the following command:

```shell
kubectl apply -f labs/03/kiali-no-auth.yaml
```


## Creating An Ingress Gateway For Istio


### Install Istio ingress gateway

```shell
# cat labs/04/ingress-gateways.yaml
apiVersion: install.istio.io/v1alpha1
kind: IstioOperator
metadata:
  name: istio-ingress-gw-install
spec:
  profile: empty
  values:
    gateways:
      istio-ingressgateway:
        autoscaleEnabled: false
  components:
    ingressGateways:
    - name: istio-ingressgateway
      namespace: istio-system
      enabled: true
      k8s:
        overlays:
        - apiVersion: apps/v1
          kind: Deployment
          name: istio-ingressgateway
          patches:
          - path: spec.template.spec.containers[name:istio-proxy].lifecycle
            value:
              preStop:
                exec:
                  command: ["sh", "-c", "sleep 5"]
```

