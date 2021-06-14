# docker registry as pull-through-cache on k8s

Manifests for running the docker registry on k8s as a pull-through cache

(adapted from [this article](https://medium.com/swlh/deploy-your-private-docker-registry-as-a-pod-in-kubernetes-f6a489bf018))

This setup works on my k3s cluster, which handles some things differently than other clusters might do (volumes, loadbalancers, ...). Adapt where necessary.

## Todo

The following things are currently not implemented in this simple demo setup.
- [] TLS for the registry
- [] Authentication for the registry

## Credentials

Get yourself a docker hub account (if you do not have one already) and create a access token. You will need to to not hit the Docker rate limiting that easily.

## Create the secret.yml file

Copy the `secret.yml.sample` file to `secret.yml` and add the credentials part:
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
- `secret.yml`
- `persistentvolumeclaim.yml`
- `deployment.yml`
- `service.yml`
- `ingress.yml`

### Deploying with a hostPath volume

In case you want to use a hostPath volume to decide where the container stores its images, use the following two files instead of the `persistentvolumeclaim.yml` file.

- `volume_with_hostpath/persistentvolumeclaim_with_storageclass.yml`
- `volume_with_hostpath/persistentvolume_with_storageclass.yml`

## Useful links
- [Docker registry documentation](https://docs.docker.com/registry/)
- [Docker registry pull-through cache](https://docs.docker.com/registry/recipes/mirror/)
- [Configure a docker registry](https://docs.docker.com/registry/configuration/)
- [Deploy Your Private Docker Registry as a Pod in Kubernetes](https://medium.com/swlh/deploy-your-private-docker-registry-as-a-pod-in-kubernetes-f6a489bf0180)
- [How to run a Public Docker Registry in Kubernetes](https://www.nearform.com/blog/how-to-run-a-public-docker-registry-in-kubernetes/)
- [Installing a Registry on Kubernetes (Quickstart)](https://blog.container-solutions.com/installing-a-registry-on-kubernetes-quickstart)
