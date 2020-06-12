#--------------------------------------------------------------
# base build image
#--------------------------------------------------------------
ARG IMAGE_VERSION=latest
FROM alpine:${IMAGE_VERSION}

#--------------------------------------------------------------
# ARG
#--------------------------------------------------------------
ARG WORKDIR=/workspace
ARG CLOUD_COMMANDS_VERSION
# https://github.com/helm/helm/releases
ARG HELM_VERSION=3.2.3

#--------------------------------------------------------------
# workdir
#--------------------------------------------------------------
RUN mkdir -p ${WORKDIR}
WORKDIR  ${WORKDIR}

#--------------------------------------------------------------
# Install dependent packages
#--------------------------------------------------------------
RUN apk update && \
    apk add --no-cache make bash tar curl zip openssl jq sudo py-pip && \
    # setting commands
    curl -sSfL https://raw.githubusercontent.com/y-miyazaki/cloud-commands/master/install.sh | sh -s -- ${CLOUD_COMMANDS_VERSION} && \
    # Install az command
    apk add --no-cache python3-dev && \
    apk add --no-cache --virtual=build gcc libffi-dev musl-dev openssl-dev  && \
    pip3 --no-cache-dir install -U pip && \
    pip3 --no-cache-dir install mock==4.0.2 && \
    pip3 --no-cache-dir install azure-cli && \
    apk del --purge build && \
    # Install kubectl(latest version)
    az aks install-cli && \
    # Install helm
    cd / && curl -OL https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    tar -zxvf helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    mv linux-amd64/helm /usr/local/bin/helm && \
    chmod +x /usr/local/bin/helm && \
    rm -f helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    # install stern
    STERN_VERSION=$(curl --silent "https://api.github.com/repos/wercker/stern/releases/latest" |  grep '"tag_name":' |  sed -E 's/.*"(v?[^"]+)".*/\1/' ) && \
    curl -LO https://github.com/wercker/stern/releases/download/${STERN_VERSION}/stern_linux_amd64 && \
    mv stern_linux_amd64 /usr/local/bin/stern && \
    chmod 775 /usr/local/bin/stern && \
    # skaffold
    curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
    sudo install skaffold /usr/local/bin/

ENTRYPOINT []
CMD ["/usr/local/bin/azdockercmd"]
