# Defining Services

Recall from the k8s primitives section, a Service is an abstraction on top of Pods, which provides a single IP address and DNS name by which the Pods can be accessed. This load balancing configuration is much easier to manage and helps scale Pods seamlessly.
Kubernetes can then provide service discovery and handle routing with the static IP for each pod as well as load balancing (round-robin based) connections to that service among the pods that match the label selector indicated.

There are 3 types of services each more complex than the last, building up to `LoadBalancer`.

1. ClusterIP [Default] - This results in a local ip that can be reached by other pods within the cluster only, end result is round robin traffic delivery.
2. NodePort - Includes a ClusterIP, but also creates an iptable rule on each node, at a designated port per service with this type, to allow external traffic.
3. LoadBalancer - Typically used with CloudProviders, this provides a ClusterIP, a NodePort, and additionally creates a cloud loadbalancer attached to your nodes on that NodePort.

The following examples can be copied into local files, and applied via `kubectl apply -f <filename>`.

Example Service #1:

```
kind: Service
apiVersion: v1
metadata:
  name: my-service1
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
```

Example Service #2:

```
kind: Service
apiVersion: v1
metadata:
  name: my-service2
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
    nodePort: 31560
  type: NodePort
```

Example Service #3: (Only works correctly with Cloud Provider K8s clusters such as AKS, not Minikube.)

```
kind: Service
apiVersion: v1
metadata:
  name: my-service3
spec:
  selector:
    app: MyApp
  ports:
  - protocol: TCP
    port: 80
    targetPort: 9376
    nodePort: 31560
  type: LoadBalancer
```


`kubectl get svc my-service1 -o yaml`

`kubectl describe svc my-service1`

### Cleanup

`kubectl get svc`

`kubectl delete svc <svc_name>`
