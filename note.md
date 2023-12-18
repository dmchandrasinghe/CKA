# Cluster Architecture, Installation & Configuration

## Understand Kubernetes components and architecture


1. Kubernetes Control Plane:

API Server: Accepts and processes requests from users and other components.

Controller Manager: Monitors the state of the cluster and takes corrective 
actions.

Scheduler: Assigns pods to worker nodes

etcd: Stores the cluster state


2. Kubernetes Worker Nodes:
Run containerized applications (pods).
Managed by the control plane.
Communicate with the control plane via the kubelet agent.

3. Pods:
The basic unit of deployment in Kubernetes.
Group one or more containers and shared storage.
Scheduled to run on worker nodes.

4. Deployments:
Manage the creation and scaling of pods.
Define a desired state for a set of pods.
Kubernetes automatically creates or deletes pods to match the desired state.

5. Services:
Provide a stable endpoint for pods.
Load balance traffic across pods in a set.
Can be exposed internally or externally to the cluster.

6. Namespaces:
Logical partitions of a Kubernetes cluster.
Isolate resources and prevent conflicts between applications.

7. Networking:
Pods can communicate with each other using IP addresses or DNS names.
Services provide a single point of access for pods.
Ingress controllers allow external traffic to reach pods.

8. Storage:
Persistent storage can be attached to pods using PersistentVolumes and PersistentVolumeClaims.
Different storage classes can be used to provision different types of storage.

9. Security:
Kubernetes provides role-based access control (RBAC) to control access to the cluster.
Pods can be run with security contexts to limit their privileges.
Network policies can be used to control traffic between pods.

10. Lables
simply key value pairs
use to metion or  describe resource (pods, nodes) own sparticular group

11. Selectors
filter pods using lables apply function (network policy,).


## Manage role-based access control (RBAC)

## Use Kubeadm to install a basic cluster

## Manage a highly-available Kubernetes cluster

## Provision underlying infrastructure to deploy a Kubernetes cluster

# #Perform a version upgrade on a Kubernetes cluster using Kubeadm

## Implement etcd backup and restore



# Workloads & Scheduling

## Understand deployments and how to perform rolling update and rollbacks

## Use ConfigMaps and Secrets to configure applications

## Know how to scale applications

## Understand the primitives used to create robust, self-healing, application deployments

## Understand how resource limits can affect Pod scheduling

## Awareness of manifest management and common templating tools






# Services & Networking

## Understand host networking configuration on the cluster nodes

## Understand connectivity between Pods

## Understand ClusterIP, NodePort, and LoadBalancer service types and endpoints

## Know how to use Ingress controllers and Ingress resources

## Know how to configure and use CoreDNS

## Choose an appropriate container network interface plugin







# Storage

## Understand storage classes, persistent volumes

## Understand volume mode, access modes and reclaim policies for volumes

## Understand persistent volume claims primitive

## Know how to configure applications with persistent storage






# Troubleshooting

## Evaluate cluster and node logging

## Understand how to monitor applications

## Manage container stdout & stderr logs

## Troubleshoot application failure

## Troubleshoot cluster component failure

## Troubleshoot networking
