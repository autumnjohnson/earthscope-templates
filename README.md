# README

Directories: 
* /earthscope-templates
    * /ingress-nginx
    * /examples
    * /prometheus
    * /grafana


## ingress-nginx

Commands for ingress-nginx:
-    `kubectl apply -f  ingress-nginx/namespace.yaml`
-    `helm add repo ingress-nginx`
-    `helm  -f ingress-nginx/values.yaml install ingress-nginx ingress-nginx/ingress-nginx -n ingress-nginx`
-    `helm show values ingress-nginx/ingress-nginx > values.yaml`
-    `helm install ingress-nginx ingress-nginx/ingress-nginx -f values.yaml`
-    `helm upgrade ingress-nginx ingress-nginx/ingress-nginx -f values.yaml`

Confirm installation or upgrade:

6.   `POD_NAME=$(kubectl get pods -l app.kubernetes.io/name=ingress-nginx -o jsonpath='{.items[0].metadata.name}') kubectl exec -it $POD_NAME -- /nginx-ingress-controller --version`
7.   `curl -D- http://www-dev1.ds.iris.edu`

To uninstall completely:

1.  `helm uninstall ingress-nginx -n ingress-nginx`

## prometheus

Steps to install prometheus:

- `kubectl apply --kustomize earthscope-templates/prometheus`

-  `helm upgrade -f values.yaml ingress-nginx ingress-nginx/ingress-nginx \`
-   --namespace ingress-nginx \`
-   --set controller.metrics.enabled=true \`
-   --set-string controller.podAnnotations."prometheus\.io/scrape"="true" \`
-   --set-string controller.podAnnotations."prometheus\.io/port"="10254"`


Confirm installation or upgrade:

-  `kubectl get svc -n ingress-nginx`

##  grafana


Steps to install prometheus:

- `kubectl apply --kustomize earthscope-templates/grafana`



https://kubernetes.github.io/ingress-nginx/user-guide/monitoring/
Todo?
Obtain the IP address of the nodes in the running cluster:

-   `kubectl get nodes -o wide`

In some cases where the node only have internal IP addresses we need to execute:

curl -r http://github.com/kubernetes/ingress-nginx/deploy/prometheus/ prometheus

## Test service http-svc
- kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/docs/examples/http-svc.yaml
- kubectl patch svc http-svc -p '{"spec":{"type": "LoadBalancer"}}'
- kubectl get svc http-svc

helm upgrade -f values.yaml ingress-nginx ingress-nginx/ingress-nginx \
--namespace ingress-nginx \
--set controller.metrics.enabled=true \
--set-string controller.podAnnotations."prometheus\.io/scrape"="true" \
--set-string controller.podAnnotations."prometheus\.io/port"="10254"
