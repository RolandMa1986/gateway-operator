## what's KubeSphere Gateway?

Gateway is a core feature of Kubesphere that used to expose cluster's internal Services to outside. Any `Ingress Controller` that serves `Ingress` could be defined as a KubeSphere Gateway. However, those third-part Ingress controller may have diffirent deployment mode and various confirurations. It hardly to deploy those controllers mannully to meet KubeSphere's requirment. 

So the `Gateway` Kind is defined, it contains a collection of common properties that used to desribe the desired status of the Gateway. An Operator can be defined to monitor the Gateway CR and creates the Ingress Contoller related resources.
 
## what's Gateway-operator?

`Gateway-operator` is an Helm Operator that used to install Gateway, an `Nginx Ingress Controller` based Gateway will be installed when a `Gateway` CR created. Under the hood, The `Gateway-operator` monitors both `Gateway` and `Nginx` Kind. The `Gateway` CR will be converted to `Nginx` through helm chart firstly, then `Gateway-operator` will be triggered again and Nginx Ingress Contoller will be deployment with the provided vaules.

## Gateway Kind definetion

```yaml
controller:
  replicaCount: 1
  annotations: {}
  ## Limit the scope of the controller
  ##
  scope:
    enabled: false
    namespace: ""  # defaults to .Release.Namespace
  image:
    repository: kubesphere/ingress-nginx-controller
    tag: "v0.48.1"
    pullPolicy: IfNotPresent

service:
  ## annotations for Services, used for config Cloud LoadBalancer
  annotations: {}
  type: LoadBalancer

## for nginx controller, same with controller
deployment:
  annotations: {}
  replicaCount: 1
```