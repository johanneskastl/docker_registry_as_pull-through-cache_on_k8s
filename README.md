# docker_registry_as_pull-through-cache_on_k8s

Manifests for running the docker registry on k8s

(adapted from [this article](https://medium.com/swlh/deploy-your-private-docker-registry-as-a-pod-in-kubernetes-f6a489bf018))

This setup works on my k3s cluster, which handles some things differently than other clusters might do (volumes, loadbalancers, ...). Adapt where necessary

## Credentials

Get yourself a docker hub account (if you do not have one already) and create a access token. You will need to to not hit the Docker rate limiting that easily.

## Create the configMap file

Copy the `configmap.yml.sample` file to `configmap.yml` and add the credentials part:
```
      remoteurl: https://registry-1.docker.io
      username: myuser
      password: my-access-token
```

## Create the ingress file

Copy the `ingress.yml.sample` file to `ingress.yml` and adjust the host line:
```
  rules:
  - host: myhostname-goes-here.example.org
    http:
```

## Deploy the things

Apply all yaml files in the following order:
- configmap.yml
- volumeclaim.yaml
- volume.yaml
- pod.yml
- service-clusterIP.yml
- ingress.yml

## Using a loadbalancer

In case you want to use a loadbalancer instead of the ingress, deploy `service-Loadbalancer.yml` instead of `service-clusterIP.yml` and do not deploy the `ingress.yml` file.
