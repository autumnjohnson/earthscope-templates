# README

Directories: 
* /earthscope-templates
    * /ingress-nginx
    * /examples
    * /cert-manager
    * /kube-prometheus
## ingress-nginx

Commands for ingress-nginx:
-    `kubectl apply -f  ingress-nginx/namespace.yaml`
-    `helm add repo ingress-nginx`
-    `helm  -f ingress-nginx/values.yaml install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx`
-    `helm show values ingress-nginx/ingress-nginx > values.yaml`
-    `helm install ingress-nginx ingress-nginx/ingress-nginx -f values.yaml`
-    `helm upgrade ingress-nginx ingress-nginx/ingress-nginx -f values.yaml`

Confirm installation or upgrade:

-   `POD_NAME=$(kubectl get pods -l app.kubernetes.io/name=ingress-nginx -o jsonpath='{.items[0].metadata.name}') kubectl exec -it $POD_NAME -- /nginx-ingress-controller --version`
-   `curl -D- http://www-dev1.ds.iris.edu`
-   `kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller`

To uninstall completely:

-  `helm uninstall ingress-nginx -n ingress-nginx


## cert-manager

### CustomResourceDefinitions
To install the CustomResourceDefinitions:
-   ` kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.1/cert-manager.crds.yaml`
-   
To uninstall the CustomResourceDefinitions (if you want to completely uninstall cert-manager):
-   `kubectl delete -f https://github.com/jetstack/cert-manager/releases/download/v1.4.1/cert-manager.crds.yaml

Commands for cert-manager:
-   `helm repo add jetstack https://charts.jetstack.io`
-   `helm repo update`
-   `kubectl apply -f cert-manager/namespace.yaml`
-   `helm show values jetstack/cert-manager > cert-manager/values.yaml`
-   `helm -f cert-manager/values.yaml install cert-manager jetstack/cert-manager --namespace cert-manager`
-   `helm upgrade cert-manager jetstack/cert-manager -f cert-manager/values.yaml -n cert-manager`

To remove:
- `helm uninstall cert-manager jetstack\cert-manager`
- `helm repo remove jetstack https://charts.jetstack.io`

To install a ClusterIssuer certificate:
-   `kubectl create -f cert-manager/cluster-issuer/letsencrypt-staging.yaml`
-   `kubectl create -f cert-manager/cluster-issuer/letsencrypt-production.yaml`

## prometheus


| Project         | Version | Repository                                               |
| --------------- | ------- | -------------------------------------------------------- |
| kubernetes      | v1.21.2 | -                                                        |
| kube-prometheus | v0.8.0  | <https://github.com/prometheus-operator/kube-prometheus> |


The kube-prometheus project bundles a whole bunch of useful libraries that together provide full-featured monitoring stack for  Kubernetes clusters.

### Directory structure

/kube-prometheus
    /manifests:     The build directory. These are deleted and regenerated after each compilation
    /vendor:        These bundled library files 

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

## Exposing monitoring services on your local machine

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.

To expose Prometheus:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/prometheus-k8s 9999:9090

Visit http://localhost:9999 on your local machine.

To expose Grafana:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/grafana 9999:3000

Visit http://localhost:9999 on your local machine.

To expose AlertManager:
$ ssh -L 9999:localhost:9999 devops@xsede-dev1.ds.iris.edu
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9999:9093

Visit http://localhost:9999 on your local machine.
