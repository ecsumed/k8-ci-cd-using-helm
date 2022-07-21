
# K8 ci/cd
A simple full-automated ci/cd deployment to k8 using helm and github actions

## Stack
* Kubernetes
* Github Actions
* [Helm](https://helm.sh/)
* [Semantic Release](https://github.com/semantic-release/semantic-release)
* Docker (and Dockerhub)
* [Snyk (for image scan)](https://snyk.io)

## Overview
**Semantic Release**
The [semantic-release](https://github.com/semantic-release/semantic-release) npm module is used to auto-increment the version. The version is used to tag the docker image and is also applied to the helm chart version.

**Docker**
For the example app, a simple nginx container is used with a hello world page. Since - for security purposes - we do not want to run the container as root, the [official nginx unpriviledged](https://hub.docker.com/r/nginxinc/nginx-unprivileged) image is used. This runs the main process as the `nginx` user as opposed to the `root`. We could have achieved the same thing with the primary nginx image, but at the expense of having to add more configuration in our Dockerfile. The unpriviledged image just keeps things simpler.

The build process uses docker/snyk to scan the image for vulnerabilities.

The image is made public just to keep a working demo example. It should not be used in production (or anywhere else besides a demo).

**Helm & K8**
Helm charts are used to deploy the app to a K8 cluster. The [helm-chart/values.yaml](helm-chart/values.yaml) file is currently kept to a minimum, but we should easily be able to parametrize many other configs (resources limits, names, etc...).

**Github Actions**
Github actions is used for the full ci/cd pipeline. The pipeline can be found at [.github/workflows/pipeline.yml](.github/workflows/pipeline.yml) which Github will automatically pick up.


The following secrets will need to be configured in the Github repo (Go to: Settings->Security->Secrets->Actions->Repository Secrets) to get the demo to work.

| Secret Name              | Purpose                                             |
|--------------------------|-----------------------------------------------------|
| DOCKER_HUB_ACCESS_TOKEN  | To push the newly built docker image                |
| DOCKER_HUB_USERNAME      | To push the newly built docker image                |
| DOCKER_HUB_SNYK_TOKEN    | [snyk.io](https://snyk.io) token to scan the image  |
| KUBECONFIG               | Used by helm to do the deployment                   |

