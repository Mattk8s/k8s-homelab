🌐 Kubernetes Home Lab – Production‑Style Cluster & Monitoring Stack
This repository contains a fully documented, production‑inspired Kubernetes home lab. It demonstrates real‑world platform engineering practices including GitOps‑style organization, observability, application deployment, storage, and cluster operations.

The goal of this project is to showcase hands‑on Kubernetes expertise suitable for cloud engineering roles, including cluster design, monitoring, troubleshooting, and application lifecycle management.

📁 Repository Structure
Code
k8s-lab/
├── apps/
│   ├── podinfo/
│   │   ├── deployment.yaml
│   │   ├── service.yaml
│   │   ├── ingress.yaml
│   │   ├── servicemonitor.yaml
│   │   └── README.md
│   └── ...
├── monitoring/
│   ├── kube-prometheus-stack/
│   ├── grafana/
│   ├── dashboards/
│   └── README.md
├── storage/
│   ├── longhorn/
│   └── README.md
└── README.md   ← (this file)
This layout mirrors GitOps conventions used in production clusters.

🚀 Cluster Overview
This Kubernetes cluster is built to emulate real‑world operational environments:

Multi‑node cluster (control plane + workers)

Container runtime: containerd

CNI: Calico

Load balancer: MetalLB

Ingress: NGINX Ingress Controller

Storage: Longhorn distributed block storage

Monitoring: Prometheus Operator + Grafana

Applications: Podinfo (demo microservice)

The cluster is designed for iterative learning, troubleshooting, and portfolio‑ready documentation.

📡 Monitoring Stack (Prometheus Operator)
The monitoring stack is deployed using kube‑prometheus‑stack, which includes:

Prometheus Operator

Prometheus

Alertmanager

Grafana

Node Exporter

kube‑state‑metrics

This setup provides:

automatic target discovery

ServiceMonitor‑based scraping

cluster‑level and application‑level metrics

production‑style dashboards

See monitoring/README.md for detailed configuration.

📦 Application: Podinfo
Podinfo is deployed as a demo microservice to validate:

Ingress routing

ServiceMonitor scraping

Prometheus metrics

Grafana dashboards

Application‑level observability

Key Features
/ – UI

/ping – increments UI counter

/metrics – Prometheus metrics endpoint

/readyz and /healthz – probes

Metrics Exposed
Podinfo 6.5.x exposes:

http_requests_total – total HTTP requests

http_request_duration_seconds_* – latency histogram

Go runtime metrics

Process metrics

Important Note About Ping Counter
The UI “Ping” counter only increments for /ping.
Prometheus metrics count all HTTP traffic, including:

/

/metrics (scraped every 15s)

/healthz

/readyz

/api_info

/ping

This is why Prometheus counters are much higher than the UI ping counter.

📊 PromQL Queries
Request Rate (RPS)
Code
sum(rate(http_requests_total[1m]))
Ping‑Only Traffic
Code
sum(rate(http_request_duration_seconds_count{path="ping"}[1m]))
Latency (P95)
Code
histogram_quantile(
  0.95,
  sum(rate(http_request_duration_seconds_bucket[5m])) by (le)
)
Pod CPU Usage
Code
sum(rate(container_cpu_usage_seconds_total{pod=~"podinfo.*"}[5m])) by (pod)

📈 Grafana Dashboards
Dashboards included:

Cluster health

Node performance

Podinfo application dashboard

Longhorn storage dashboard

Custom panels for request rate, latency, and pod health

Dashboards are stored under:

Code
monitoring/dashboards/

🧪 Troubleshooting Notes
This repo documents real troubleshooting scenarios, including:

ServiceMonitor not scraping

Missing application metrics

kubelet vs. app‑level metrics

Longhorn metrics integration

Prometheus scrape path validation

In‑cluster debugging using BusyBox

Example validation command:

Code
kubectl run tmp --rm -it --image=busybox -- sh
wget -qO- http://podinfo.apps.svc.cluster.local:80/metrics

🧭 Goals of This Project
This repository demonstrates:

Kubernetes operational maturity

GitOps‑style repo structure

Monitoring and observability expertise

Real‑world troubleshooting

Production‑grade documentation

Application‑level metrics integration

Storage and networking fundamentals

It is designed to serve as a portfolio centerpiece for cloud engineering roles.

📌 Next Steps
Add CI/CD (GitHub Actions or ArgoCD)

Add more microservices

Add autoscaling (HPA + metrics)

Add logging stack (Loki or EFK)

Add synthetic monitoring (Blackbox Exporter)

Add cluster architecture diagram

🏁 How to Deploy
Code
kubectl apply -f monitoring/
kubectl apply -f storage/
kubectl apply -f apps/

📬 Contact / Notes
This repo is actively maintained as part of ongoing Kubernetes learning and professional development.