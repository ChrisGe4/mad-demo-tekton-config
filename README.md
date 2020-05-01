# MAD Tekton Pipeline Example

This guide walks through a detailed example that demonstrates using a Tekton pipeline to build, push, hydrate and deploy an application in a Git repository to a GKE cluster.

```
* Build image and tag it with "dev".
* Hydrate k8s manifests and upload them to GCS.
* Download the k8s manifests from GCS and Deploy the service.
```


## Set up your Tekton cluster

1. Follow the instructions [here](https://github.com/tektoncd/pipeline/blob/master/docs/install.md#installing-tekton-pipelines-1) to install Tekton Pipelines onto your cluster.
   
  ```bash

  # Install Tekton Pipelines
  kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml

  # Install CLI
  sudo tar xvzf tkn_0.8.0_Darwin_x86_64.tar.gz -C /usr/local/bin/ tkn

  # Give service account the permission to modify the cluster  
    kubectl create clusterrolebinding demo-binding \
             --clusterrole=edit \
             --serviceaccount=default:default

  # Start up the dashboard
    kubectl apply --filename https://github.com/tektoncd/dashboard/releases/latest/download/tekton-dashboard-release.yaml
    kubectl --namespace tekton-pipelines port-forward svc/tekton-dashboard 9097:9097

  ```

## Run Pipeline

1. Install the task and pipeline.

  ```bash
  kubectl apply -f task-build.yaml
  kubectl apply -f task-hydrate.yaml
  kubectl apply -f task-deploy.yaml
  kubectl apply -f pipeline-build-hydrate-deploy.yaml.yaml

  ```

2. Run the Pipeline.

  ```bash
  kubectl apply -f pipelinerun-dev.yaml
  ```

3. Watch the logs.

  ```bash
  tkn pipelinerun logs demo-pipeline -f
  ```

  or go to localhost:9097 