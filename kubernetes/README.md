# Wave - Kubernetes Deployment Guide

Complete Kubernetes deployment guide for the CleanStart Wave container. Wave is a Kubernetes controller/operator that watches and manages Deployments, DaemonSets, and StatefulSets across the cluster.

**What Wave Does:**
- Monitors Kubernetes resources (Deployments, DaemonSets, StatefulSets)
- Watches for changes in ConfigMaps and Secrets
- Provides metrics on port 8080 via `/metrics` endpoint
- Operates as a cluster-scoped controller

## Files

- `deployment.yaml` - Complete deployment manifest (Namespace, ServiceAccount, ClusterRole, ClusterRoleBinding, Deployment, Service)
- `README.md` - This documentation

## Image Details

**Image:** `cleanstart/wave:latest-dev`

**Key Features:**
- **Binary Location:** `/usr/bin/wave`
- **Command:** The deployment uses `command: ["/usr/bin/wave"]` to run the application
- **User:** `clnstrt` (non-root, UID 1000)
- **Architecture:** `amd64`
- **OS:** `linux`
- **SSL Certificates:** Pre-configured at `/etc/ssl/certs/ca-certificates.crt`

## Complete Deployment Steps

### Prerequisites

1. **Kubernetes cluster** (Kind, minikube, k3s, GKE, EKS, AKS, or any other)
2. **kubectl** installed and configured to access your cluster

### Step 1: Verify Kubernetes Cluster

```bash
# Check cluster connectivity
kubectl cluster-info

# Verify nodes are ready
kubectl get nodes
```

**Expected Output:**
```
NAME                 STATUS   ROLES           AGE   VERSION
kind-control-plane   Ready    control-plane   5m    v1.27.3
```

### Step 2: Deploy Wave

```bash
# Navigate to the deployment directory
cd containers/wave/kubernetes

# Apply the deployment
kubectl apply -f deployment.yaml
```

**Expected Output:**
```
namespace/wave created
serviceaccount/wave created
clusterrole.rbac.authorization.k8s.io/wave created
clusterrolebinding.rbac.authorization.k8s.io/wave created
deployment.apps/wave created
service/wave created
```

### Step 3: Verify Deployment

```bash
# Check if the namespace was created
kubectl get namespace wave

# Watch the pod status (press Ctrl+C to exit)
kubectl get pods -n wave -w
```

**Expected Output:**
```
NAME                    READY   STATUS    RESTARTS   AGE
wave-xxxxxxxxxx-xxxxx   1/1     Running   0          30s
```

### Step 4: Check Pod Logs

```bash
# Get pod name
POD_NAME=$(kubectl get pods -n wave -l app=wave -o jsonpath='{.items[0].metadata.name}')

# Follow logs in real-time
kubectl logs -n wave -f $POD_NAME
```

### Step 5: Verify Service

```bash
# Check service
kubectl get svc -n wave

# Get service details
kubectl describe svc wave -n wave
```

**Expected Output:**
```
NAME   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
wave   ClusterIP   10.96.xxx.xxx   <none>        8080/TCP   1m
```

### Step 6: Test Functionality

#### Option A: Port Forward (Recommended for Testing)

```bash
# Forward local port 8080 to pod port 8080
kubectl port-forward -n wave svc/wave 8080:8080

# In another terminal, test connectivity
# Wave serves metrics on /metrics endpoint
curl http://localhost:8080/metrics

# Or open metrics endpoint in browser
http://localhost:8080/metrics
```

#### Option B: Exec into Pod

```bash
# Get pod name
POD_NAME=$(kubectl get pods -n wave -l app=wave -o jsonpath='{.items[0].metadata.name}')

# Exec into the pod
kubectl exec -it -n wave $POD_NAME -- /bin/sh

# Inside the pod, test the wave binary
wave --help
# or
wave --version
# or check what commands are available
```

#### Option C: Check Container Image Details

```bash
# Describe the pod to see image details
kubectl describe pod -n wave -l app=wave | grep -A 5 Image

# Check container image digest
kubectl get pod -n wave -l app=wave -o jsonpath='{.items[0].status.containerStatuses[0].imageID}'
```

## 🔐 RBAC Permissions

Wave is a Kubernetes controller/operator that requires RBAC permissions to function properly. The deployment includes:

**ClusterRole Permissions:**
- **Core resources** (secrets, configmaps, pods, services, endpoints, events, namespaces): get, list, watch, create, update, patch
- **Apps resources** (deployments, daemonsets, statefulsets, replicasets): get, list, watch, create, update, patch
- **Leader election** (leases): get, list, watch, create, update, patch

These permissions allow Wave to:
- Watch for changes in deployments, daemonsets, and statefulsets
- Access and modify secrets and configmaps
- Perform leader election for high availability deployments

### Pod Not Starting

```bash
# Check pod events
kubectl describe pod -n wave -l app=wave

# Check for image pull errors
kubectl get events -n wave --sort-by='.lastTimestamp'
```

### Check Container Resources

```bash
# Check resource usage
kubectl top pod -n wave

# Check resource limits
kubectl describe pod -n wave -l app=wave | grep -A 5 "Limits\|Requests"
```

## 🧹 Cleanup

```bash
# Delete the deployment
kubectl delete -f deployment.yaml

# Or delete namespace (this will delete everything in the namespace)
kubectl delete namespace wave

# Verify deletion
kubectl get namespace wave
```
