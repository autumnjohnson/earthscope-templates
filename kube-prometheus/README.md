# README
 
| Project         | Version | Repository                                               |
| --------------- | ------- | -------------------------------------------------------- |
| kubernetes      | v1.21.2 | -                                                        |
| kube-prometheus | v0.8.0  | <https://github.com/prometheus-operator/kube-prometheus> |

## Overview

The kube-prometheus project bundles a whole bunch of useful libraries that together provide full-featured monitoring stack for  Kubernetes clusters.

### Updating the project

To update the  kube-prometheus dependency (and its constituent dependencies) simply use the jsonnet-bundler update functionality:

> jb update

The following packages, which are part of kube-prometheus, will be updated:

- <https://github.com/prometheus-operator/kube-prometheus>
- <https://github.com/kubernetes-monitoring/kubernetes-mixin>
- <https://github.com/etcd-io/etcd>
- <https://github.com/prometheus-operator/prometheus-operator>
- <https://github.com/prometheus-operator/prometheus-operator>
- <https://github.com/kubernetes/kube-state-metrics>
- <https://github.com/kubernetes/kube-state-metrics>
- <https://github.com/prometheus/node_exporter>
- <https://github.com/prometheus/prometheus>
- <https://github.com/prometheus/alertmanager>
- <https://github.com/brancz/kubernetes-grafana>
- <https://github.com/thanos-io/thanos>
- <https://github.com/ksonnet/ksonnet-lib>
- <https://github.com/grafana/grafonnet-lib>
- <https://github.com/grafana/jsonnet-libs>
- <https://github.com/kubernetes-monitoring/kubernetes-mixin>

### Compiling the project

> ./build.sh earthscope-monitoring.jsonnet

## Installing manifests in a Kubernetes cluster

### Create the namespace and CRDs, and then wait for them to be available before creating the remaining resources

> kubectl create -f manifests/setup
> until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
> kubectl create -f manifests/

### Update the namespace and CRDs, and then wait for them to be available before creating the remaining resources

> kubectl apply -f manifests/setup
> kubectl apply -f manifests/

### Delete stack

> kubectl delete --ignore-not-found=true -f manifests/ -f manifests/setup