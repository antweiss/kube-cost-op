title: The Kubernetes Cost Optimization Guide

# Pod Autoscaling

Autoscaling is a widely accepted practice in cloud computing which entails automatically adjusting the amount of computational resources based on load.

Autoscaling automates resource management and as such has a significant impact on cluster costs. 

Well configured autoscaling can make your cluster lean, efficient and reliable. 

Badly configured autoscaling can generate waste and reduce availability.

Kubernetes comes with add-ons that allow us to autoscale **pods** in 2 distinct ways:

1. Horizontally

    By changing the number of pod replicas in a workload (Deployment, StatefulSet) 

2. Vertically

    By changing the amount of resources (CPU, memory) allocated to each individual pod in a workload.


## Horizontal Pod Autoscaling

Horizontal pod autoscaling is enabled by the following add-ons:

*(Follow the links for autoscaling optimization recommendations)*

- [HPA](../hpa)
- [KEDA](../keda)

## Vertical Pod Autoscaling

Horizontal pod autosscaling is enabled by the following add-ons:

*(Follow the links for autoscaling optimization recommendations)*

- [VPA](../vpa)
- [Goldilocks](../goldilocks)