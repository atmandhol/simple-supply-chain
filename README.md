## Simple Supply chain


### Cluster Setup

```bash
## Create a kind cluster
kind create cluster --name sc-test

## Install cert-manager. Install docs https://cert-manager.io/docs/installation/
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml

## Install Cartographer from release pages https://github.com/vmware-tanzu/cartographer/releases
kubectl apply -f https://github.com/vmware-tanzu/cartographer/releases/download/v0.7.2/cartographer.yaml

## Install Tekton from Install page https://tekton.dev/docs/installation/pipelines/
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

## Install kpack from https://github.com/pivotal/kpack/blob/main/docs/install.md
kubectl apply -f https://github.com/pivotal/kpack/releases/download/v0.9.4/release-0.9.4.yaml

## Follow the kpack tutorial to setup the rest of kpack
## https://github.com/pivotal/kpack/blob/main/docs/tutorial.md

## Install Flux
kubectl apply -f https://github.com/fluxcd/flux2/releases/latest/download/install.yaml
```

### Apply Supplychain, rbac and template manifests
```
kubectl apply -f rbac
kubectl apply -f templates
kubectl apply -f supplychain
kubectl apply -f kpack
```

### Create a Registry secret
```
kubectl create secret docker-registry dockerhub/gcr \
    --docker-username=user \
    --docker-password=password \
    --docker-server=string \
    --namespace default
```

### Create a workload using apps plugin and simple supply chain
```
tanzu apps wld apply petc \
--image springcommunity/spring-framework-petclinic \
--label workload-type="pre-built" \
--tail \
--yes
```

### Create a workload using apps plugin and fancy supply chain
```
tanzu apps wld apply tanzu-java-web-app \
--git-repo https://github.com/sample-accelerators/tanzu-java-web-app \
--git-branch main \
--label workload-type="from-git" \
--tail \
--yes
```

### Delete the workload
```
tanzu apps wld delete tanzu-java-web-app --yes
```


