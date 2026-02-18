# Kubernetes Architecture

## Overview
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

## Architecture Components

### Control Plane (Master Node)
The control plane manages the Kubernetes cluster and makes global decisions.

#### 1. API Server (kube-apiserver)
- Central management entity and entry point for all REST commands
- Validates and processes API requests
- Updates etcd with cluster state

#### 2. etcd
- Distributed key-value store
- Stores all cluster data and configuration
- Provides backup and restore capabilities

#### 3. Scheduler (kube-scheduler)
- Assigns pods to nodes based on resource requirements
- Considers constraints, affinity rules, and available resources
- Ensures optimal workload distribution

#### 4. Controller Manager (kube-controller-manager)
- Runs controller processes that regulate cluster state
- **Node Controller**: Monitors node health
- **Replication Controller**: Maintains correct pod count
- **Endpoints Controller**: Manages service endpoints
- **Service Account Controller**: Creates default accounts

#### 5. Cloud Controller Manager
- Manages cloud-specific control logic
- Handles node, route, service, and volume controllers

### Worker Nodes
Worker nodes run containerized applications.

#### 1. Kubelet
- Agent running on each node
- Ensures containers are running in pods
- Communicates with API server
- Reports node and pod status

#### 2. Kube-proxy
- Network proxy maintaining network rules
- Enables service abstraction and load balancing
- Handles pod-to-pod and external communication

#### 3. Container Runtime
- Software responsible for running containers
- Supports Docker, containerd, CRI-O
- Pulls images and manages container lifecycle

## Key Objects

### Pod
- Smallest deployable unit
- Contains one or more containers
- Shares network and storage resources

### Service
- Abstracts pod access with stable endpoint
- Types: ClusterIP, NodePort, LoadBalancer, ExternalName

### Deployment
- Manages pod replicas and updates
- Enables rolling updates and rollbacks

### Namespace
- Virtual clusters for resource isolation
- Organizes objects within a cluster

### ConfigMap & Secret
- **ConfigMap**: Stores non-sensitive configuration
- **Secret**: Stores sensitive data (passwords, tokens)

### Volume
- Persistent storage for pods
- Survives container restarts

## Communication Flow

1. User sends request to API Server
2. API Server validates and stores in etcd
3. Scheduler assigns pods to nodes
4. Kubelet on node pulls container images
5. Container runtime starts containers
6. Kube-proxy configures networking
7. Controllers monitor and maintain desired state

## Architecture Diagram

```
┌─────────────────────────────────────────────────┐
│              Control Plane (Master)              │
│  ┌──────────┐  ┌──────┐  ┌───────────┐         │
│  │   API    │  │ etcd │  │ Scheduler │         │
│  │  Server  │  └──────┘  └───────────┘         │
│  └──────────┘                                    │
│  ┌─────────────────────┐  ┌──────────────────┐ │
│  │ Controller Manager  │  │ Cloud Controller │ │
│  └─────────────────────┘  └──────────────────┘ │
└─────────────────────────────────────────────────┘
                      │
        ┌─────────────┴─────────────┐
        │                           │
┌───────▼────────┐         ┌────────▼───────┐
│  Worker Node 1 │         │  Worker Node 2 │
│  ┌──────────┐  │         │  ┌──────────┐  │
│  │ Kubelet  │  │         │  │ Kubelet  │  │
│  └──────────┘  │         │  └──────────┘  │
│  ┌──────────┐  │         │  ┌──────────┐  │
│  │Kube-proxy│  │         │  │Kube-proxy│  │
│  └──────────┘  │         │  └──────────┘  │
│  ┌──────────┐  │         │  ┌──────────┐  │
│  │Container │  │         │  │Container │  │
│  │ Runtime  │  │         │  │ Runtime  │  │
│  └──────────┘  │         │  └──────────┘  │
│  ┌──────────┐  │         │  ┌──────────┐  │
│  │   Pods   │  │         │  │   Pods   │  │
│  └──────────┘  │         │  └──────────┘  │
└────────────────┘         └────────────────┘
```

## Benefits

- **High Availability**: Automatic failover and self-healing
- **Scalability**: Horizontal and vertical scaling
- **Portability**: Runs on any infrastructure
- **Declarative Configuration**: Desired state management
- **Service Discovery**: Automatic DNS and load balancing

## Getting Started

```bash
# Check cluster info
kubectl cluster-info

# View nodes
kubectl get nodes

# Deploy application
kubectl create deployment nginx --image=nginx

# Expose service
kubectl expose deployment nginx --port=80 --type=LoadBalancer

# Scale deployment
kubectl scale deployment nginx --replicas=3
```

## Resources

- [Official Documentation](https://kubernetes.io/docs/)
- [Kubernetes GitHub](https://github.com/kubernetes/kubernetes)
- [Interactive Tutorial](https://kubernetes.io/docs/tutorials/)
