# Traefik Ingress Example

## Prerequisite

- docker
- [mkcer](https://github.com/FiloSottile/mkcert)

## Setup

### Create TLS secret

``` shell
mkcert whoami.traefik.me
```


### Run service
```shell
docker compose up -d
```
```

### View whoami from ingress enpoint

Go to [whoami.traefik.me](whoami.traefik.me) link to view whoami