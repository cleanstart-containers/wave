# Wave - CleanStart Container

A containerized Kubernetes controller/operator that monitors and manages workloads across your cluster.

## Overview

Wave is a Kubernetes operator that watches and manages key cluster resources, providing automation and monitoring capabilities for Deployments, DaemonSets, and StatefulSets. It operates as a cluster-scoped controller, continuously monitoring resource changes and maintaining desired state across namespaces.

**Key Features:**
* Kubernetes-native controller built on controller-runtime framework
* Cluster-wide monitoring of Deployments, DaemonSets, and StatefulSets
* ConfigMap and Secret change detection
* Prometheus-compatible metrics endpoint for observability
* Leader election support for high availability deployments

**Common Use Cases:**
* Automated workload management and reconciliation
* Cluster-wide resource monitoring and automation
* Workload synchronization and state management
* Infrastructure as Code (IaC) operations in Kubernetes
* Multi-tenant cluster resource management

## What Wave Does

Wave operates as a Kubernetes operator that:

1. **Monitors Workloads**: Continuously watches Deployments, DaemonSets, and StatefulSets across all namespaces in the cluster
2. **Tracks Configuration**: Monitors ConfigMaps and Secrets associated with workloads for configuration changes
3. **Maintains State**: Ensures workloads maintain their desired state and reconciles any drift
4. **Provides Metrics**: Exposes Prometheus-style metrics on port 8080 at the `/metrics` endpoint for monitoring and observability
5. **Leader Election**: Supports leader election for high availability deployments where only one instance is active at a time

## Image Details

**Image:** `cleanstart/wave:latest-dev`

**Key Specifications:**
* **Binary Location:** `/usr/bin/wave`
* **User:** `clnstrt` (non-root, UID 1000)
* **Architecture:** `amd64`
* **OS:** `linux`
* **SSL Certificates:** Pre-configured at `/etc/ssl/certs/ca-certificates.crt`

## How It Works

Wave is deployed as a Kubernetes controller that:

1. **RBAC Integration**: Uses ClusterRole and ClusterRoleBinding to access cluster resources across all namespaces
2. **Event Watching**: Establishes watches on Deployments, DaemonSets, and StatefulSets to detect changes
3. **Configuration Monitoring**: Tracks associated ConfigMaps and Secrets for configuration drift detection
4. **Reconciliation Loop**: Continuously reconciles the current state with the desired state
5. **Metrics Collection**: Collects and exposes operational metrics for monitoring

## Kubernetes Deployment

The `kubernetes/` directory contains a complete, production-ready Kubernetes deployment:

## Configuration

### Ports

* **8080** - Metrics endpoint (TCP) - Serves Prometheus-style metrics at `/metrics`

### Environment Variables

* `SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt` - SSL certificate path for secure communication
* `PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` - Standard PATH environment variable

### RBAC Permissions

Wave requires cluster-scoped permissions to:
* Watch and list Deployments, DaemonSets, StatefulSets, and ReplicaSets
* Access ConfigMaps and Secrets across the cluster
* Manage leader election leases
* Read Pod, Service, Endpoint, Event, and Namespace resources

## Best Practices

* Deploy Wave in a dedicated namespace with proper RBAC configuration
* Configure resource limits to ensure predictable resource usage
* Enable leader election for high availability deployments
* Monitor the metrics endpoint for operational insights
* Use least privilege principle for RBAC permissions
* Keep the container image updated with the latest security patches
* Implement proper network policies if required by your security policies

## Security Notes

* The container runs as non-root user (`clnstrt`, UID 1000)
* All Linux capabilities are dropped for security
* RBAC is configured with least privilege principle (cluster-scoped access only where necessary)
* SSL certificates are pre-configured for secure communication
* Leader election ensures only one instance is active at a time, reducing attack surface
* Security context prevents privilege escalation

## Observability

Wave exposes operational metrics that can be scraped by Prometheus or other monitoring systems:
* Controller runtime metrics
* Workload reconciliation metrics
* Event processing metrics
* Resource watch metrics

Metrics are available at the `/metrics` endpoint on port 8080 when accessed via port-forward or service.

For deployment instructions, testing procedures, and troubleshooting guides, see the `kubernetes/README.md` file in the `kubernetes/` directory.

