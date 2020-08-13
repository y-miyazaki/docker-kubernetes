# VS Code Remote Development for Kubernetes

## Overview

This repository is for building an environment targeting AWS EKS.  
This environment uses [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) and install kubectl/helm/stern/skaffold/AWS CLI

## Description

[VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) can use VS Code and Docker technology to create a Docker container from VS Code and enable work in the Docker container with VS Code.  
If you have a base Docker image, you can complete all work with a Docker container without building an environment locally.  
The base Docker image is already uploaded to the [Docker Hub] (https://hub.docker.com/) repository and is ready to use.

https://hub.docker.com/r/ymiyazakixyz/kubernetes-aws

### Set devcontainer.json

Set devcontainer.json to use [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview).

```bash
$ cp -rp env/aws/template env/aws/{your environment}
$ cat env/aws/{your environment}/.devcontainer/devcontainer.json
{
  "image": "registry.hub.docker.com/ymiyazakixyz/kubernetes-aws:latest",
  "settings": {
    "terminal.integrated.shell.linux": "/bin/bash"
  },
  "extensions": [
    "ms-kubernetes-tools.vscode-kubernetes-tools",
    "ms-azuretools.vscode-docker",
    "coenraads.bracket-pair-colorizer-2",
    "eamodio.gitlens",
    "editorconfig.editorconfig",
    "esbenp.prettier-vscode",
    "ibm.output-colorizer",
    "streetsidesoftware.code-spell-checker",
    "vscode-icons-team.vscode-icons",
  ],
  "build": {
    "args": {
      "WORKDIR": "/workspace"
    }
  },
  "runArgs": [
    "-v",
    "${env:HOME}/##YOUR_WORKSPACE##:/workspace",
    "--env-file=.env"
  ],
  "workspaceFolder": "/workspace",
  "overrideCommand": false
}
```

### change devcontainer.json

"##YOUR_WORKSPACE##" fix in devcontainer.json.

1. ##YOUR_WORKSPACE## is your local directory for volume mount. If you want to absolute directory, don't need to \${env:HOME} value.

```json
{
  "image": "registry.hub.docker.com/ymiyazakixyz/kubernetes-aws:latest",
  "settings": {
    "terminal.integrated.shell.linux": "/bin/bash"
  },
  "extensions": [
    "ms-kubernetes-tools.vscode-kubernetes-tools",
    "ms-azuretools.vscode-docker",
    "coenraads.bracket-pair-colorizer-2",
    "eamodio.gitlens",
    "editorconfig.editorconfig",
    "esbenp.prettier-vscode",
    "ibm.output-colorizer",
    "streetsidesoftware.code-spell-checker",
    "vscode-icons-team.vscode-icons",
  ],
  "build": {
    "args": {
      "WORKDIR": "/workspace"
    }
  },
  "runArgs": [
    "-v",
    "${env:HOME}/workspace/kubernetes:/workspace",
    "--env-file=.env"
  ],
  "workspaceFolder": "/workspace",
  "overrideCommand": false
}
```

### fix env/aws/{your environment}/.env.

By modifying .env, you can automatically log in to the cloud environment and automatically generate a terraform template for the provider.

```bash
$ cat env/aws/{your environment}/.env
#---------------------------------------------------------
# base
#---------------------------------------------------------
# Variable for judging the environment
ENV={development|staging|production..etc}

#---------------------------------------------------------
# This is an environment variable required to log in with the aws command.
#---------------------------------------------------------
# AWS_ACCESS_KEY_ID uses for default profile.
AWS_ACCESS_KEY_ID={aws access key}

# AWS_SECRET_ACCESS_KEY uses for default profile.
AWS_SECRET_ACCESS_KEY={aws secret key}

# AWS_DEFAULT_REGION uses for default profile.
AWS_DEFAULT_REGION={aws default region}

# AWS_REGION uses for default profile.
AWS_REGION={aws default region}
```

Here is example.

```bash
$ cat env/aws/{your environment}/.env
#---------------------------------------------------------
# base
#---------------------------------------------------------
# Variable for judging the environment
ENV=production

#---------------------------------------------------------
# This is an environment variable required to log in with the aws command.
#---------------------------------------------------------
# AWS_ACCESS_KEY_ID uses for default profile.
AWS_ACCESS_KEY_ID=xxxxxxxxxxxxxxxxxxxxxxxxx

# AWS_SECRET_ACCESS_KEY uses for default profile.
AWS_SECRET_ACCESS_KEY=xxxxxxxxxxxxxxxxxxxxxxxxx

# AWS_DEFAULT_REGION uses for default profile.
AWS_DEFAULT_REGION=ap-northeast-1

# AWS_REGION uses for default profile.
AWS_REGION=ap-northeast-1
```

### set kubeconfig
After you create your Amazon EKS cluster, you must then configure your kubeconfig file with the AWS Command Line Interface (AWS CLI). This configuration allows you to connect to your cluster using the kubectl command line.  
https://aws.amazon.com/jp/premiumsupport/knowledge-center/eks-cluster-connection/

```
# aws eks --region {your cluster region} update-kubeconfig --name {cluster name}
```

```
bash-5.0# aws eks --region ap-northeast-1 update-kubeconfig --name test
Added new context arn:aws:eks:ap-northeast-1:xxxxxxxxxxxxxx:cluster/test to /root/.kube/config
bash-5.0# kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   172.20.0.1   <none>        443/TCP   5h42m
```

### aws version

```
bash-5.0# aws --version
aws-cli/2.0.22 Python/3.7.3 Linux/4.19.76-linuxkit botocore/2.0.0dev26
```

### kubectl version

```
bash-5.0# kubectl version
Client Version: version.Info{Major:"1", Minor:"16+", GitVersion:"v1.16.8-eks-e16311", GitCommit:"e163110a04dcb2f39c3325af96d019b4925419eb", GitTreeState:"clean", BuildDate:"2020-03-27T22:40:13Z", GoVersion:"go1.13.8", Compiler:"gc", Platform:"linux/amd64"}
```

### helm version

```
bash-5.0# helm version
version.BuildInfo{Version:"v3.2.3", GitCommit:"8f832046e258e2cb800894579b1b3b50c2d83492", GitTreeState:"clean", GoVersion:"go1.13.12"}
```

### stern version

```
bash-5.0# stern -v
stern version 1.11.0
```

### skaffold version

```
bash-5.0# skaffold version
v1.11.0
```

## Required

- Visual Code Studio  
  https://code.visualstudio.com/download
- Docker  
  https://www.docker.com/

## Other Link

- Docker  
https://www.docker.com/
- AWS CLI  
https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
- Kubernetes  
https://kubernetes.io/ja/
- helm  
https://helm.sh/
- Stern  
https://github.com/wercker/stern
- Skaffold  
https://skaffold.dev/

## Note
