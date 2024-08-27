title: The Kubernetes Cost Optimization Guide

# The Economics of Kubernetes Resource Allocation

This whole guide talks a lot about *resources* which is a highly overloaded word in Kubernetes world. All the objects defined in Kubernetes API (such as Pod, Service, ConfigMap, etc) are also called *resources*. But we're not referring to them here.

So in order to make things clearer let's define resources for the purpose of this guide.

When we say *resources* - we actually mean CPU, GPU, memory, network and storage. 

In the pre-cloud world all these needed to be defined in advance. Ordering and provisioning these resources took weeks or even months.

In cloud native environments (i.e Kubernetes) these resources are highly dynamic in nature and can be allocated and released on demand - just by issuing an API call. The process of allocating additional resources in such an automated manner is called autoscaling.
This automation gives us a lot of power by prividing access to addtional resources when needed (aka just-in-time provisioning). And it also creates undesirable artifacts if not configured correctly such as:

- Wasted resources (when we allocate more than we actually need)

- Reliability issues (when provisioning doesn't work as expected)

- Unexpected costs (when we don't have good control over what and when gets provisioned)


## Resource Allocation in Kubernetes

Kubernetes has different ways of allocating, limiting and provisioning resources for application containers.

### CPU and memory allocation
On the very basic level - engineers can request CPU and memory for a container by defining its resource requests and limits in the Pod resource spec:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example
spec:
  containers:
  - image: perfectscale.io/example
    name: example
    resources: 
      requests:
        cpu: 1
        memory: 500Mi
      limits:
        cpu: 1
        memory: 500Mi
```

### Local ephemeral storage

Since v1.25 we can also manage the amount of local epehemeral storage allocated to pods.

"Ephemeral" means that there is no long-term guarantee about durability.

Pods use ephemeral local storage for scratch space, caching, and for logs. The kubelet can provide scratch space to Pods using local ephemeral storage to mount emptyDir volumes into containers.

The kubelet also uses this kind of storage to hold node-level container logs, container images, and the writable layers of running containers.

An example of a pod with ephemeral storage resource defintions:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
  - name: app
    image: perfectscale.io/my-app:v0.2
    resources:
      requests:
        ephemeral-storage: "1Gi"
      limits:
        ephemeral-storage: "2Gi"
    volumeMounts:
    - name: ephemeral
      mountPath: "/tmp"
  - name: log-collector
    image: perfectscale.io/logger:v0.4a
    resources:
      requests:
        ephemeral-storage: "500Mi"
      limits:
        ephemeral-storage: "1Gi"
    volumeMounts:
    - name: ephemeral
      mountPath: "/tmp"
  volumes:
    - name: ephemeral
      emptyDir:
        sizeLimit: 500Mi
```
For more details on managing ephemeral storage read [the official guide](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#local-ephemeral-storage)

### Persistent Storage Resource Allocation

In order to allocate storage Kubernetes allows us to use its PersistentVolume allocation mechanisms in conjunction with one of the multiple supported storage providers (e.g OpenEBS, Portworx, etc.)

### Network Resource Allocation 

Network resources aren't managed by Kubernetes itself but instead are delegated to one of the multiple CNI networking providers, which are reponsible for provisioning, allocating and retiring network interfaces and addresses. 

Continue [here](../over-under-idle-waste) to get a better understanding of the [4 main focus areas of Kubernetes Cost Optimization](../over-under-idle-waste).