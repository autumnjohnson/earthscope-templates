# README

Directories: 
* /earthscope-templates
    * /ingress-nginx
    * /examples
    * /cert-manager
     
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

To uninstall completely:

-  `helm uninstall ingress-nginx -n ingress-nginx


## cert-manager

### CustomResourceDefinitions
To install the CustomResourceDefinitions:
-   ` kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.4.1/cert-manager.crds.yaml`
        customresourcedefinition.apiextensions.k8s.io/certificaterequests.cert-manager.io created
        customresourcedefinition.apiextensions.k8s.io/certificates.cert-manager.io created
        customresourcedefinition.apiextensions.k8s.io/challenges.acme.cert-manager.io created
        customresourcedefinition.apiextensions.k8s.io/clusterissuers.cert-manager.io created
        customresourcedefinition.apiextensions.k8s.io/issuers.cert-manager.io created
        customresourcedefinition.apiextensions.k8s.io/orders.acme.cert-manager.io created

To uninstall the CustomResourceDefinitions (if you want to completely uninstall cert-manager):
-   `kubectl delete -f https://github.com/jetstack/cert-manager/releases/download/v1.4.1/cert-manager.crds.yaml

Commands for cert-manager:
-   `helm repo add jetstack https://charts.jetstack.io`
-   `helm repo update`
-   `kubectl apply -f cert-manager/namespace.yaml`
-   `helm show values jetstack/cert-manager > cert-manager/values.yaml`
-   `helm -f cert-manager/values.yaml install cert-manager jetstack/cert-manager --namespace cert-manager`

To remove:
- `helm repo remove jetstack https://charts.jetstack.io`

## Test service http-svc
- kubectl create -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/docs/examples/http-svc.yaml
- kubectl patch svc http-svc -p '{"spec":{"type": "LoadBalancer"}}'
- kubectl get svc http-svc

helm upgrade -f values.yaml ingress-nginx ingress-nginx/ingress-nginx \
--namespace ingress-nginx \
--set controller.metrics.enabled=true \
--set-string controller.podAnnotations."prometheus\.io/scrape"="true" \
--set-string controller.podAnnotations."prometheus\.io/port"="10254"
