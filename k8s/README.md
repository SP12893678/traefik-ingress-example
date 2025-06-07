# Traefik Ingress Example

## Prerequisite

- docker
- helm
- kind
- [mkcert](https://github.com/FiloSottile/mkcert)

## Setup

### Create cluster

```shell
kind create cluster --name demo --config kind-config.yml
```

### Traefik stack Installation

```shell
helm repo add traefik https://traefik.github.io/charts
helm repo update

helm install traefik traefik/traefik \
  --namespace=traefik --create-namespace \
  --set service.type=ClusterIP \
  --set ports.web.hostPort=80 \
  --set ports.websecure.hostPort=443 \
  --set ports.web.expose.enabled=true \
  --set ports.websecure.expose.enabled=true
```

### Prometheus stack Installation

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

### Create TLS secret

``` shell
mkcert grafana.traefik.me

kubectl create secret tls grafana-cert-secret \
  --cert=grafana.traefik.me.pem \
  --key=grafana.traefik.me-key.pem \
  -n monitoring
```


### Create Grafana ingress 
General Ingress way
```shell
kubectl apply -f grafana-ingress.yml
```

Traefik Ingress route CRD way
```shell
kubectl apply -f grafana-ingress-route.yml
```

### View Grafana from ingress enpoint

Go to [grafana.traefik.me](grafana.traefik.me) link to view grafana