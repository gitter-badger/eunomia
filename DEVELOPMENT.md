# Development

## Prerequisites

In order to build Eunomia, you'll need the following:

- [Git](https://git-scm.com/downloads)
- [Go](https://golang.org/dl/)
- [Dep](https://golang.github.io/dep/docs/installation.html)
- [Docker](https://docs.docker.com/install/)
- [Operator SDK](https://github.com/operator-framework/operator-sdk)
- Access to a Kubernetes cluster
  - [Minikube](https://kubernetes.io/docs/setup/minikube/) (optional)
  - [Minishift](https://www.okd.io/minishift/) (optional)

All the comonents can easily be installed via [Homebrew](https://brew.sh/) on a Mac:

```shell
brew install git
brew install go
brew install dep
brew install docker
brew install operator-sdk
brew install minikube
brew install minishift
```

## Building

### Local development

See https://golang.org/doc/install to install/setup your Go Programming environment if you have not already done this.

Run "dep ensure" before building code. (if this is your first time running this use "dep ensure -v" ,this will take some time to complete.)

```shell
dep ensure
GOOS=linux operator-sdk build eunomia-operator
```

### Remote registry

Run the following to build and push the images:

```shell
export REGISTRY=<your registry>
docker login $REGISTRY
./build-images.sh
```

## Testing

### Using Minikube

Here are some preliminary instructions. This still needs a lot of TLC. Feel free to send in PRs.

```shell
minikube start
kubectl create namespace eunomia
kubectl apply -f ./deploy/kubernetes/crds/gitops_v1alpha1_gitopsconfig_crd.yaml -n eunomia
kubectl delete configmap gitops-templates -n eunomia
kubectl create configmap gitops-templates --from-file=./templates/cronjob.yaml --from-file=./templates/job.yaml -n eunomia
kubectl apply -f ./deploy/kubernetes -n eunomia
```

## Using Openshift

Here are some preliminary instructions. This still needs a lot of TLC. Feel free to send in PRs.

```shell
oc create namespace eunomia
oc apply -f ./deploy/kubernetes/crds/gitops_v1alpha1_gitopsconfig_crd.yaml -n eunomia
oc delete configmap gitops-templates -n eunomia
oc create configmap gitops-templates --from-file=./templates/cronjob.yaml --from-file=./templates/job.yaml -n eunomia
oc apply -f ./deploy/kubernetes -f ./deploy/openshift -n eunomia

## Run Tests

`Unit Tests`

```shell
./unit-tests.sh
```

`E2E Tests`

```shell
oc login -u <username> <server>
./e2e-tests.sh
```
