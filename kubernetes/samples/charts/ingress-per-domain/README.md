# Ingress per Domain Chart
This chart is to deploy an Ingress resource in front of a WebLogic domain cluster. We support two Ingress types: Traeafik and Voyager.

## Prerequisites
- Have Docker and a Kubernetes cluster running and have kubectl installed and configured.
- Have Helm installed.
- The corresponding Ingress controller: Traefik or Voyager, is installed in the Kubernetes cluster.
- A WebLogic domain cluster deployed by weblogic-operator is running in the Kubernetes cluster.

## Installing the Chart

To install the chart with the release name my-ingress with given values.yaml:
```
# Change directory to the cloned git weblogic-kubernetes-operator repo.
$ cd kubernetes/samples/charts
$ helm install ingress-per-domain --name my-ingress --value values.yaml
```
The Ingress resource will be created in the same namespace as the WebLogic domain cluster.

Sample values.yaml for Ingress of Traefik:
```
type: TRAEFIK 

# WLS domain as backend to the load balancer
wlsDomain:
  namespace: default
  svcName: domain1-cluster-cluster-1
  svcPort: 8001

# Traefik specific values
traefik:
  # hostname used by host-routing
  hostname: doamin1.org
```

Sample values.yaml for Ingress of Voyager:
```
type: VOYAGER 

# WLS domain as backend to the load balancer
wlsDomain:
  namespace: default
  svcName: domain1-cluster-cluster-1
  svcPort: 8001

# Voyager specific values
voyager:
  # web port
  webPort: 30305
  # stats port
  statsPort: 30315
```
## Uninstalling the Chart
To uninstall/delete the my-ingress deployment:
```
$ helm delete --purge my-ingress
```
## Configuration
The following table lists the configurable parameters of this chart and their default values.

| Parameter                              | Description                                                                                                                  | Default                                           |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `type`                     | Type of Ingress controller. Legal values are "TRAEFIK" or "VOYAGER"                                                                                             | TRAEFIK |
| `wlsDomain.namespace`                     | Namespace of the WLS domain cluster.                                                                                            | default |
| `wlsDomain.svcName`                     | Service name of the WLS domain cluster.                                                                                            | domain1-cluster-cluster-1 |
| `wlsDomain.svcPort`                     | Service port of the WLS domain cluster.                                                                                            | 8001 |
| `traefik.hostname`                     | Hostname to route to the WLS domain cluster.                                                                                            | doamin1.org |
| `voyager.webPort`                     | Web port to access the Voyager load balancer.                                                                                         | 30305 |
| `voyager.statsPort`                     | Port to access the Voyager/HAProxy stats page.                                                                                            | 30315 |