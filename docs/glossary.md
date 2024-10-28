# Glossary of Kubernetes and DevOps Terms

### Ansible

An open-source automation tool used for configuration management, application deployment, and task automation. It uses a simple, declarative language (YAML) to describe system configurations.

### Kubernetes (k8s)

An open-source platform designed to automate deploying, scaling, and operating containerized applications. It groups containers that make up an application into logical units for easy management and discovery.

### k3s

A lightweight Kubernetes distribution developed by Rancher Labs. It is optimized for resource-constrained environments and edge computing.

### HA

High Availability (HA) refers to a system’s ability to maintain continuous operational performance, minimizing downtime by using redundant components, such as multiple control plane nodes and load balancers in Kubernetes, to ensure resilience even if individual components fail.

### Helm

A package manager for Kubernetes that simplifies the deployment and management of applications by using charts (pre-configured application resources).

### Longhorn

An open-source, cloud-native distributed block storage system for Kubernetes. It provides persistent storage for stateful applications.

### PersistentVolume (PV)

A piece of storage in the cluster that has been provisioned by an administrator or dynamically provisioned using Storage Classes. It is a resource in the cluster, just like a node.

### PersistentVolumeClaim (PVC)

A request for storage by a user. It is similar to a pod in that pods consume node resources and PVCs consume PV resources.

### StorageClass

Provides a way for administrators to describe the "classes" of storage they offer. Different classes might map to quality-of-service levels, backup policies, or arbitrary policies determined by the cluster administrators.

### Playbook

In Ansible, a playbook is a YAML file containing a series of tasks that define automation across a set of hosts.

### Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate.

### SSH (Secure Shell)

A cryptographic network protocol for operating network services securely over an unsecured network. It is commonly used for remote command-line login and remote command execution.

### YAML (YAML Ain't Markup Language)

A human-readable data serialization language commonly used for configuration files and in applications where data is being stored or transmitted.

### Idempotency

A property of certain operations in mathematics and computer science whereby they can be applied multiple times without changing the result beyond the initial application.

### Infrastructure as Code (IaC)

The process of managing and provisioning computing infrastructure through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.

### Control Plane

The collection of processes that control Kubernetes nodes. This includes the kube-apiserver, etcd (cluster data store), kube-scheduler, and kube-controller-manager.

### Node

A worker machine in Kubernetes, which may be a virtual machine or a physical machine, depending on the cluster.

### Pod

The smallest and simplest unit in the Kubernetes object model that you create or deploy. A Pod represents a running process on your cluster and can contain one or more containers.

### Service

An abstraction which defines a logical set of Pods and a policy by which to access them, sometimes called a micro-service.

### Namespace

A Kubernetes namespace provides a mechanism to isolate groups of resources within a single cluster.

### LoadBalancer

A type of Kubernetes service that exposes the service externally using a cloud provider's load balancer.

### Ingress

An API object that manages external access to the services in a cluster, typically HTTP.

---

