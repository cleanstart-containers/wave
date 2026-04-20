# Wave - CleanStart Container

A containerized Kubernetes controller/operator that monitors and manages workloads across your cluster. The CleanStart Wave image provides a production-ready, security-hardened container optimized for enterprise environments. Built on a minimal base OS with comprehensive security hardening, this image delivers reliable application execution with advanced security features.

**📌 CleanStart Foundation:** Security-hardened, minimal base OS designed for enterprise containerized environments.

**Image Path:** `ghcr.io/cleanstart-containers/wave`

**Registry:** CleanStart Registry

---

## Overview

Wave is a Kubernetes operator that watches and manages key cluster resources, providing automation and monitoring capabilities for Deployments, DaemonSets, and StatefulSets. It operates as a cluster-scoped controller, continuously monitoring resource changes and maintaining desired state across namespaces. This Wave container is part of the CleanStart application suite, featuring enterprise-grade security hardening, automated vulnerability management, and compliance with industry standards.

**Image:** `ghcr.io/cleanstart-containers/wave:latest-dev`

**Key Specifications:**
- **Binary Location:** `/usr/bin/wave`
- **User:** `clnstrt` (non-root, UID 1000)
- **Architecture:** `amd64`
- **OS:** `linux`
- **SSL Certificates:** Pre-configured at `/etc/ssl/certs/ca-certificates.crt`

---

## About CleanStart

CleanStart is a comprehensive container registry providing security-hardened, enterprise-ready container images. Our images are designed with security-first principles, featuring minimal attack surfaces, regular security updates, and compliance with industry standards.

### About CleanStart Images

CleanStart images are built on secure, minimal base operating systems and optimized for production environments. Each image undergoes rigorous security testing, vulnerability scanning, and compliance validation to ensure enterprise-grade security and reliability.

---

## Key Features

- **Security-First Design**: Built with minimal attack surfaces and security hardening
- **Enterprise Compliance**: Meets industry standards including FIPS, STIG, and CIS benchmarks
- **Regular Updates**: Automated security patches and vulnerability management
- **Multi-Architecture Support**: Available for AMD64 and ARM64 architectures
- **Production Ready**: Optimized for enterprise deployment and scaling
- **Comprehensive Documentation**: Detailed guides and best practices for each image
- Kubernetes-native controller built on controller-runtime framework
- Cluster-wide monitoring of Deployments, DaemonSets, and StatefulSets
- ConfigMap and Secret change detection
- Prometheus-compatible metrics endpoint for observability
- Leader election support for high availability deployments

---

## Common Use Cases

Typical scenarios where this container excels:

- Automated workload management and reconciliation
- Cluster-wide resource monitoring and automation
- Workload synchronization and state management
- Infrastructure as Code (IaC) operations in Kubernetes
- Multi-tenant cluster resource management
- Enterprise deployment and scaling operations

---

## What Wave Does

Wave operates as a Kubernetes operator that:

1. **Monitors Workloads**: Continuously watches Deployments, DaemonSets, and StatefulSets across all namespaces in the cluster
2. **Tracks Configuration**: Monitors ConfigMaps and Secrets associated with workloads for configuration changes
3. **Maintains State**: Ensures workloads maintain their desired state and reconciles any drift
4. **Provides Metrics**: Exposes Prometheus-style metrics on port 8080 at the `/metrics` endpoint for monitoring and observability
5. **Leader Election**: Supports leader election for high availability deployments where only one instance is active at a time

---

## How It Works

Wave is deployed as a Kubernetes controller that:

1. **RBAC Integration**: Uses ClusterRole and ClusterRoleBinding to access cluster resources across all namespaces
2. **Event Watching**: Establishes watches on Deployments, DaemonSets, and StatefulSets to detect changes
3. **Configuration Monitoring**: Tracks associated ConfigMaps and Secrets for configuration drift detection
4. **Reconciliation Loop**: Continuously reconciles the current state with the desired state
5. **Metrics Collection**: Collects and exposes operational metrics for monitoring

---

## Quick Start

### Pull Commands
```bash
docker pull ghcr.io/cleanstart-containers/wave:latest
docker pull ghcr.io/cleanstart-containers/wave:latest-dev
```

### Run Commands

Basic run:
```bash
docker run -it --name wave-test ghcr.io/cleanstart-containers/wave:latest-dev
```

Production deployment:
```bash
docker run -d --name wave-prod \
  --read-only \
  --security-opt=no-new-privileges \
  --user 1000:1000 \
  ghcr.io/cleanstart-containers/wave:latest
```

---

## Configuration

### Ports

- **8080** - Metrics endpoint (TCP) - Serves Prometheus-style metrics at `/metrics`

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `SSL_CERT_FILE` | `/etc/ssl/certs/ca-certificates.crt` | SSL certificate path for secure communication |
| `PATH` | `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` | Standard PATH environment variable |

### RBAC Permissions

Wave requires cluster-scoped permissions to:

- Watch and list Deployments, DaemonSets, StatefulSets, and ReplicaSets
- Access ConfigMaps and Secrets across the cluster
- Manage leader election leases
- Read Pod, Service, Endpoint, Event, and Namespace resources

---

## Best Practices

- Deploy Wave in a dedicated namespace with proper RBAC configuration
- Configure resource limits to ensure predictable resource usage
- Enable leader election for high availability deployments
- Monitor the metrics endpoint for operational insights
- Use least privilege principle for RBAC permissions
- Keep the container image updated with the latest security patches
- Implement proper network policies if required by your security policies

---

## Security Notes

The container runs with multiple security enhancements:

- The container runs as non-root user (`clnstrt`, UID 1000)
- All Linux capabilities are dropped for security
- RBAC is configured with least privilege principle (cluster-scoped access only where necessary)
- SSL certificates are pre-configured for secure communication
- Leader election ensures only one instance is active at a time, reducing attack surface
- Security context prevents privilege escalation

---

## Observability

Wave exposes operational metrics that can be scraped by Prometheus or other monitoring systems:

- Controller runtime metrics
- Workload reconciliation metrics
- Event processing metrics
- Resource watch metrics

Metrics are available at the `/metrics` endpoint on port 8080 when accessed via port-forward or service.

---

## Architecture Support

CleanStart images support multiple architectures to ensure compatibility across different deployment environments:

- **AMD64**: Intel and AMD x86-64 processors
- **ARM64**: ARM-based processors including Apple Silicon and ARM servers

### Architecture-based Pull Commands
```bash
docker pull --platform linux/amd64 ghcr.io/cleanstart-containers/wave:latest
docker pull --platform linux/arm64 ghcr.io/cleanstart-containers/wave:latest
```

---

## Kubernetes Deployment

The `kubernetes/` directory contains a complete, production-ready Kubernetes deployment.

For deployment instructions, testing procedures, and troubleshooting guides, see the `kubernetes/README.md` file in the `kubernetes/` directory.

---

## Documentation Resources
Essential links and resources for further information
 
**CleanStart Images**: https://images.cleanstart.com/
 
**Community Images**:<br>
**Docker Hub**: https://hub.docker.com/u/cleanstart<br>
**GitHub**: https://github.com/cleanstart-containers<br>
**AWS ECR Public Gallery**: https://gallery.ecr.aws/cleanstart/
 
**Presence on Social Media**:<br>
**Community**: https://www.linkedin.com/groups/18324021/<br>
**YouTube**: https://www.youtube.com/@CleanStartOfficial<br>
 
**Contribute to Container Use Cases**: https://github.com/cleanstart-dev/cleanstart-use-cases/

## Disclaimer & License

### Disclaimer

**Disclaimer:** This documentation is provided for informational purposes only. Users are responsible for ensuring compliance with applicable laws, regulations, and security requirements. CleanStart makes no warranties regarding the suitability of these images for specific use cases or environments.

### License

Apache-2.0

---

## Vulnerability Disclaimer

CleanStart offers Docker images that include third-party open-source libraries and packages maintained by independent contributors. While CleanStart maintains these images and applies industry-standard security practices, it cannot guarantee the security or integrity of upstream components beyond its control.

Users acknowledge and agree that open-source software may contain undiscovered vulnerabilities or introduce new risks through updates. CleanStart shall not be liable for security issues originating from third-party libraries, including but not limited to zero-day exploits, supply chain attacks, or contributor-introduced risks.

**Security remains a shared responsibility:** CleanStart provides updated images and guidance where possible, while users are responsible for evaluating deployments and implementing appropriate controls.
