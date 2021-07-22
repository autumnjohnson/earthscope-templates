 helm show values ingress-nginx/ingress-nginx > values.yaml

helm install  ingress-nginx ingress-nginx/ingress-nginx -f values.yaml
 helm upgrade ingress-nginx ingress-nginx/ingress-nginx -f values.yaml