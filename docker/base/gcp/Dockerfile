#--------------------------------------------------------------
# base build image
#--------------------------------------------------------------
ARG TERRAFORM_VERSION=latest
FROM hashicorp/terraform:${TERRAFORM_VERSION}

#--------------------------------------------------------------
# ARG
#--------------------------------------------------------------
ARG WORKDIR=/workspace
ARG CLOUD_COMMANDS_VERSION
# https://github.com/helm/helm/releases
ARG HELM_VERSION=3.2.3

#--------------------------------------------------------------
# ENV
#--------------------------------------------------------------
ENV CLOUDSDK_PYTHON=python3

#--------------------------------------------------------------
# workdir
#--------------------------------------------------------------
RUN mkdir -p ${WORKDIR}
WORKDIR  ${WORKDIR}

#--------------------------------------------------------------
# Install dependent packages
#--------------------------------------------------------------
RUN apk update && \
    apk add --no-cache git make bash tar curl zip openssl jq sudo python3 && \
    # setting commands
    curl -sSfL https://raw.githubusercontent.com/y-miyazaki/cloud-commands/master/install.sh | sh -s -- ${CLOUD_COMMANDS_VERSION} && \
    # Install Google Cloud SDK
    curl https://sdk.cloud.google.com | bash && \
    $SHELL && \
    echo "source /root/google-cloud-sdk/path.bash.inc" >> $HOME/.bashrc && \
    echo "source /root/google-cloud-sdk/completion.bash.inc" >> $HOME/.bashrc && \
    # Install kubectl
    /root/google-cloud-sdk/bin/gcloud components install kubectl && \
    # Install helm
    cd / && curl -OL https://get.helm.sh/helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    tar -zxvf helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    mv linux-amd64/helm /usr/local/bin/helm && \
    chmod +x /usr/local/bin/helm && \
    rm -f helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    # Install stern
    STERN_VERSION=$(curl --silent "https://api.github.com/repos/wercker/stern/releases/latest" |  grep '"tag_name":' |  sed -E 's/.*"(v?[^"]+)".*/\1/' ) && \
    curl -LO https://github.com/wercker/stern/releases/download/${STERN_VERSION}/stern_linux_amd64 && \
    mv stern_linux_amd64 /usr/local/bin/stern && \
    chmod 775 /usr/local/bin/stern && \
    # Install skaffold
    curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
    sudo install skaffold /usr/local/bin/

ENTRYPOINT []
CMD ["/usr/local/bin/gcpdockercmd"]
