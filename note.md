# Cluster Architecture, Installation & Configuration

## Understand Kubernetes components and architecture

1. Kubernetes Control Plane:

    API Server: Accepts and processes requests from users and other components.

    Controller Manager: Monitors the state of the cluster and takes corrective
actions.

        * Scheduler: Assigns pods to worker nodes

        * etcd: Stores the cluster state

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
    use to metion or describe resource (pods, nodes) own sparticular group

11. Selectors

    filter pods using lables apply function (network policy, deployments).

12. workloads

    type of workload :  

      * replica-sets : maintain the set of pods as metions in pod yaml

      * deployment : create pods
        deployment --> replicaSet --> Pods

      * daemonSet :  create pods in each node in the cluster (onitoring agent,log tools)

      * StatefulSet :
            Maintain state of pods even in recreated. Usefull when creating clustered applications (Database, Redis, etc).

          * pods need persitant state (ex: map storage to pod)
          * start pods in ordered list

      * Jobs : Pod need run and stop do specific task

      * cronjobs : same as job but execute as defined schedule

13. Services

    access the pods using unifed method. create single interface to all the pods which serve single task.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      # Optional field
      # By default and for convenience, the Kubernetes control plane will allocate a port from a range (default: 30000-32767)
      nodePort: 30007
```

how to call service

`<service name>.<namespace>.svc.cluster.local`

type of services to communicate

1. CLsuterIP :
commubicate with in the cluster using pod networking

  ```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: review-deployment
  labels:
    app: review
spec:
  replicas: 3
  selector:
    matchLabels:
      app: review
  template:
    metadata:
      labels:
        app: review
    spec:
      containers:
        - name: review-app
          image: chamilaliyanage/echo-env-http:1.1
          ports:
            - containerPort: 8080
          env:
            - name: MY_POD_IP
              valueFrom:
                fieldRef:
                  fieldPath: status.podIP
            - name: MY_APP_NAME
              value: review-application
---
kind: Service
apiVersion: v1
metadata:
  name: review-service
spec:
  selector:
    app: review
  ports:
    - port: 80
      targetPort: 8080

```

2. NodePOrt :
extend cluster service to external network (node network)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  type: NodePort
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080 # app's container port
      nodePort: 30000 # pecify a specific nodePort or Kubernetes will assign one

```

3. LoadBalancer : distributes incoming traffic among your app's pods and is cloud-provider-specific. 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080 #  app's container port

```
4. ExternalName: 

```yaml
apiversion: v1
kind: service
metadata: 
  name: example-externalName
spec:
  type: ExternalName
  externalName: rds.aws.com
```


## Manage role-based access control (RBAC)

* shoud know how to create, modify and delete RBACs.

## Use Kubeadm to install a basic cluster

* should be able to operate the kubeadm tool to set up a Kubernetes cluster

## Manage a highly-available Kubernetes cluster

* should be able to understand how to add nodes to the cluster and configure it to be highly available

## Provision underlying infrastructure to deploy a Kubernetes cluster

* main goal here is to be able to lay the groundwork for a Kubernetes cluster installation (network, storage, dependencies, etc.)

Installing Kubeadm
Create a cluster Kubeadm
For kubernetes to work, you need to have

Certain system configurations
Container runtime (CRI-O, Containerd, or Docker)
kubeadm
kubelet and kubectl

## Perform a version upgrade on a Kubernetes cluster using Kubeadm

* will be asked to upgrade a Kubernetes cluster using Kubeadm.

upgrade Kubeadm Cluster

## Implement etcd backup and restore

should learn and practice using the etcdctl utility to backup and restore etcd.

Etcd is the cluster’s key-value store. All cluster configuration and information about pods, services, and so on are stored in key-value format here.

etcd Backup & Restore Operations

# Workloads & Scheduling

## Understand deployments and how to perform rolling update and rollbacks

Kubernetes Deployment ensures that an application has a minimum number of replicas running at all times. In the event that a replica fails, the Kubernetes API ensures that a new one is created within minutes.

In the Exam , you should know how to do rollbacks and rollouts of deployments.

Kubernetes Deployment Concepts
Kubernetes Rolling Update

## Use ConfigMaps and Secrets to configure applications

Configmaps in Kubernetes are useful for storing non-critical data in key-value pair format. They can also be used to inject environment variables into pods.

In the Exam , you should knwo how to use configmaps and secrets objects to create, modify, and delete variables and secrets and make them available to a pod.

Kubernetes Configmap Concepts
Kubernetes Secrets Concepts

## Know how to scale applications

Kubernetes offers a variety of ways to scale applications, including the use of deployment objects to increase the number of replicas of your application.

Horizontal Pod Autoscalers (HPAs) can be used to increase the number of replicas based on application metrics.

For the Exam , you should be able to scale a pod/deployment. You can follow this tutorial.

Working With Horizontal Pod Autoscaler

## Understand the primitives used to create robust, self-healing, application deployments

For any self-healing application, you should use deployments or stateful sets so that when pods fail, Kubernetes instantly recreates them.

Deployments also allow you to keep track of all the changes you make. You can also easily return to a previous state.

Kubernetes Deployments

## Understand how resource limits can affect Pod scheduling

Cluster management also includes workload management; as an administrator, you should ensure that each pod has access to resources based on its requirements.

Each pod in kubernetes can be assigned a minimum and maximum CPU and memory usage.

Manage Container Resources
Pods with resource requests

## Awareness of manifest management and common templating tools

This section assumes you’re familiar with tools like kustomization, helm, and so on.

In general , during the Exam , you should be able to create, modify and apply Kubernetes manifests

Managing Kubernetes Objects
Manage objects with Kustomize

# Services & Networking

## Understand host networking configuration on the cluster nodes

Kube-proxy is a component that must be installed on each worker node in order for pods to communicate with one another. Kube proxy participation is required for node networking.

Kubelet is the process by which a worker node communicates with the master node. All of these concepts are required to comprehend networking within Kubernetes.

Kubernetes Networking

## Understand connectivity between Pods

Pods communicate with one another via services. This is made possible by the Kube proxy component.

Understand Kube Proxy

## Understand ClusterIP, NodePort, and LoadBalancer service types and endpoints

Understanding each service type and their use cases is critical. Understanding how pods can be added to a service should be given special consideration.

Kubernetes Service
External Resource Exposure

## Know how to use Ingress controllers and Ingress resources

External entities are granted access to internal cluster services via ingress resources. Ingress controllers are load balancers that enable it.

For the Exam , you should know how to create and configure Ingress Understand Ingress Controllers

Kubernetes Ingress
Kubernetes Ingress Controller
Ingress TLS/SSL Setup

## Know how to configure and use CoreDNS

CoreDNS is a highly adaptable and extensible DNS server that can act as the Kubernetes cluster DNS. The CNCF hosts the CoreDNS project, as it does Kubernetes.

Using CoreDNS for Service Discovery

## Choose an appropriate container network interface plugin

The Container Networking Interface (CNI) aims to develop a generic plugin-based networking solution for containers.

For the Exam , you should Know how to choose a CNI according to your needs.

There are numerous options, including Flannel, Calico, and others.

Kubernetes Network Plugins
The network section accounts for 20% of the exam’s content. You’ll almost certainly be asked to create at least one network policy, endpoint, or ingress.

# Storage

## Understand storage classes, persistent volumes

Kubernetes Persistent Volumes

## Understand volume mode, access modes and reclaim policies for volumes

Kubernetes Volume Modes
Kubernetes Volume Access Modes

## Understand persistent volume claims primitive

Persistent Volumes Claims

## Know how to configure applications with persistent storage

By mounting a PVC, application pods can use persistent storage.

Configure Kubernetes Volume in Pod

# Troubleshooting

## Evaluate cluster and node logging

Application logs can aid in understanding the application’s activities and status. The logs are especially useful for troubleshooting and monitoring cluster activity.

Examining logs of Kubernetes control plane components such as etcd and the scheduler can also be very beneficial.

Kubernetes Logging

## Understand how to monitor applications

Monitoring applications can be accomplished by storing logs and analyzing application metrics.

Tools like Prometheus and Grafana are popular because they make metric management simple.

Monitoring & Logging & Debugging

## Manage container stdout & stderr logs

Create Logging agent (stdeout & stderr )

## Troubleshoot application failure

Administrators should also assist users in debugging applications that have been deployed into Kubernetes but are not behaving correctly.

Understanding Kubernetes Logging
Debug Kubernetes Objects

## Troubleshoot cluster component failure

When users are confident that their application is properly configured, cluster components must be debugged and troubleshooted for failures.

Debug Kubernetes Cluster

## Troubleshoot networking

There may be instances where things go wrong on the network end, such as incorrect configuration of ingress resources.

Cluster Networking

references: <https://teckbootcamps.com/cka-exam-study-guide/>
