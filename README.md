# Hands-on-Lab.Portable-Grafana-Lab

## Introduction

This repository provides a **portable**, **ready-to-run lab environment** to learn and master Grafana, Prometheus, and observability workflows. It includes pre-configured setups for Grafana dashboards, Prometheus monitoring, MySQL metrics collection, and SMTP alert notifications, available for both **Docker Compose** and **Kubernetes(Kind)** environments. For more advanced Grafana dashboard configuration and management techniques, please take a look at [Grafana-in-Action](https://github.com/Rurutia1027/Grafana-in-Action). Users can build on this foundation for minor customizations or further development. 


## Lab Structure 

```
grafana-lab/
│
├── docker/                 # Docker Compose setup
│   ├── docker-compose.yml
│   ├── grafana/
│   │   └── data/           # Persistent Grafana volume
│   ├── prometheus/
│   │   └── prometheus.yml
│   ├── mysql/
│   └── mailhog/            # Local SMTP for alerts
│
├── k8s/                    # Kubernetes (Kind) setup
│   ├── kind-config.yaml    # Cluster configuration
│   ├── manifests/
│   │   ├── grafana-deployment.yaml
│   │   ├── grafana-service.yaml
│   │   ├── prometheus-deployment.yaml
│   │   ├── prometheus-service.yaml
│   │   ├── mysql-deployment.yaml
│   │   ├── mysql-service.yaml
│   │   └── mailhog-deployment.yaml
│   └── volumes/            # Optional PV/PVC for Grafana & Prometheus
│
├── ci/                     # CI/CD pipeline configs
│   └── ci-pipeline.yaml
│
└── README.md
```

- Docker and K8S are isolated to prevent config file conflicts.
- Persistent volumes ensure dashboard data and metrics are retained across restarts.

## Requirements 
- Linux or MacOS environment
- Docker >= 20.x / Docker COmpose >= 1.29
- Kind (for Kubernetes local cluster)
- kubectl >= 1.27
- Internet connection (for pulling images)

## Getting Started 
### Docker Compose Setup 

- Navigate to docker/ folder:
```bash
cd docker
docker-compose -d 
```

- Grafana: `http://localhost:3000 (admin/admin)`
- Prometheus: `http://localhost:9090`
- MySQL metrics exporter `http://localhost:9104`
- MailHog (SMTP test) `http://localhost:8025`

Volumes are mapped for Grafana data and Prometheus configuration.

### Kubernetes (Kind) Setup 
- Create a local Kind cluster
```bash
kind create cluster --name grafana-lab --config kind-config.yaml 
```

- Apply manifests
```bash
kubectl apply -f k8s/manifests/
```

- Access services via **NodePort** (mapped in `kind-config.yaml`)
```
Grafana: http://localhost:32000

Prometheus: http://localhost:31000

MySQL Exporter: http://localhost:30092

MailHog: http://localhost:32080
```

## Available Dashboards & Metrics 
- MySQL: `threads_connected`, `queries`, `uptime`
- NodeExporter: CPU, Memory, Disk, Network
- Prometheus: Up metrics, custom application metrics
- SMTP: MailHog logs for alert verification

## SMTP Alerts 
- MailHog is provided as a local SMTP server for testing Grafana alerts
- All notifications are  configured in Grafana, alerting will be sent to MailHog
- Web UI: `http://localhost:8025`

## Further Reading 
- Grafana Documentation
- Prometheus Documentation
- Node Exporter
- Kind Kubernetes
