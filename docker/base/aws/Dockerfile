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
ARG GLIBC_VER=2.31-r0
ARG KUBECTL_VERSION=1.16.8/2020-04-16
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
    apk add --no-cache make bash tar curl zip openssl jq sudo && \
    # fix sudo: setrlimit(RLIMIT_CORE): Operation not permitted
    echo "Set disable_coredump false" >> /etc/sudo.conf && \
    # setting commands
    curl -sSfL https://raw.githubusercontent.com/y-miyazaki/cloud-commands/master/install.sh | sh -s -- ${CLOUD_COMMANDS_VERSION} && \
    # install glibc compatibility for alpine(need to add it for AWS CLI)
    apk --no-cache add binutils && \
    curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk && \
    curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk && \
    # Install AWS CLI
    apk add --no-cache glibc-${GLIBC_VER}.apk glibc-bin-${GLIBC_VER}.apk && \
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" && \
    unzip awscliv2.zip && \
    sudo ./aws/install && \
    rm -rf awscliv2.zip aws && \
    bash -c 'echo complete -C '/usr/local/bin/aws_completer' aws  >> $HOME/.bashrc' && \
    # remove glibc files.
    apk --no-cache del binutils && \
    rm glibc-${GLIBC_VER}.apk && \
    rm glibc-bin-${GLIBC_VER}.apk && \
    # install kubectl
    # https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
    curl -o /usr/local/bin/kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
    chmod +x /usr/local/bin/kubectl && \
    # install eksctl
    curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp && \
    sudo mv /tmp/eksctl /usr/local/bin && \
    # install helm
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
    # Install skaffold
    curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
    sudo install skaffold /usr/local/bin/

ENTRYPOINT []
CMD ["/usr/local/bin/awsdockercmd"]
