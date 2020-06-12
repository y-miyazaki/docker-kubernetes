# VS Code Remote Development for Kubernetes

## Overview

This repository is for building an environment targeting AWS EKS.  
This environment uses [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview) and install kubectl/helm/stern/skaffold/Google Cloud SDK CLI

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

### aws version

```
bash-5.0# az --version
aws-cli                          2.7.0

command-modules-nspkg              2.0.3
core                               2.7.0
nspkg                              3.0.4
telemetry                          1.0.4

Python location '/usr/bin/python3'
Extensions directory '/root/.aws/cliextensions'

Python (Linux) 3.8.3 (default, May 15 2020, 01:53:50) 
[GCC 9.3.0]

Legal docs and information: aka.ms/awsCliLegal


Your CLI is up-to-date.

Please let us know how we are doing: https://aka.ms/clihats
and let us know if you're interested in trying out our newest features: https://aka.ms/CLIUXstudy
```

### kubectl version

```
bash-5.0# kubectl version
Client Version: version.Info{Major:"1", Minor:"18", GitVersion:"v1.18.3", GitCommit:"2e7996e3e2712684bc73f0dec0200d64eec7fe40", GitTreeState:"clean", BuildDate:"2020-05-20T12:52:00Z", GoVersion:"go1.13.9", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
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
v1.10.1
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
