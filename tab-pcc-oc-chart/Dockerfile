FROM registry.fedoraproject.org/fedora-toolbox:34
ENV NAME=cloudsecurity-fedora-toolbox VERSION=34 USER=devops
LABEL com.github.containers.toolbox="true" \
      com.github.debarshiray.toolbox="true" \
      com.redhat.component="$NAME" \
      name="$FGC/$NAME" \
      version="$VERSION" \
      usage="This image is meant to be used with the toolbox command, but can run as independent OCI Container." \
      summary="IBM MSS Cloud Security DevSecOps toolbox OCI image." \
      author="Filipe Spencer Lopes <filipe.spencerlopes@be.ibm.com>, Arvid Van Essche <arvid.van.essche@be.ibm.com>" \
      maintainer="Cerberus Squad, CloudTribe, IBM Managed Security Services"

## Create local user, not running as root
RUN groupadd -g 1001 "$USER" \
    && useradd -m -u 1001 -g 1001 "$USER"

WORKDIR /home/"$USER"

COPY README.md ./

## Update all packages
RUN dnf update --assumeyes && dnf clean all --assumeyes
RUN dnf install zsh --assumeyes && dnf clean all --assumeyes


## Set up Python
# Make a default virtual env that allows us to install ansible from PIP,
# so we have tighter version control and easier plugin management down the line.
RUN dnf install --assumeyes \
        python3-virtualenv \ 
        python3-pip \
        python3-wheel \
    && dnf clean all --assumeyes
# Set Python Virtual env location
ENV VIRTUAL_ENV=/opt/venv
# Create the virtual env
RUN python3 -m venv $VIRTUAL_ENV
# Add the virtual env to the PATH, and make it more important than original path
ENV PATH="${VIRTUAL_ENV}/bin:${PATH}"
# Update virtual env PIP to latest version
RUN pip install --upgrade pip wheel

# Install our python packages, ansible and depencies mainly.
# Add more ansible dependencies here. 
RUN pip install ansible jmespath

## Install docker CLI (Testing with RedHat's Podman)
# If Podman does not work, we should fallback on official docker releases:
# https://docs.docker.com/engine/install/fedora/
RUN dnf install podman podman-docker podman-remote --assumeyes \
 && dnf clean all --assumeyes

## Install Buildah
# Tool for building images
RUN dnf install buildah --assumeyes  && dnf clean all --assumeyes

## Install openshift module
RUN pip install openshift pyyaml

## Install IBM Cloud Tools (latest)
ENV IBMCLOUD_VERSION=latest
RUN mkdir /tmp/ibmcloud \
    && cd /tmp/ibmcloud \
    && curl -L https://clis.cloud.ibm.com/download/bluemix-cli/${IBMCLOUD_VERSION}/Linux64 -o ibmcloud-installer.tar.gz \
    && tar -xzvf ibmcloud-installer.tar.gz \
    && cd Bluemix_CLI \
    && ./install \
    && rm -rf /tmp/ibmcloud \
    && ibmcloud plugin update --all \ 
    && ibmcloud plugin install container-registry \
    && ibmcloud plugin install container-service \
    && cp -r /root/.bluemix /home/"$USER"/ \
    && chown -R "$USER":"$USER" /home/"$USER"/.bluemix

# Install Helm v3
WORKDIR /usr/local/bin
RUN rm -f helm \
    && curl -sL https://get.helm.sh/helm-v3.6.2-linux-amd64.tar.gz | tar -xz && mv linux-amd64/helm ./helm \
    && rm -rf linux-amd64 \
    && helm repo add stable https://charts.helm.sh/stable \
    && helm repo update

# Install OC + upgrade kubectl
RUN curl -sL https://mirror.openshift.com/pub/openshift-v4/clients/oc/latest/linux/oc.tar.gz | tar -xz 

# Return home
WORKDIR /home/"$USER"

## Changed to unprivileged 
USER "$USER"

## Done! - ZSH Shell as default.
CMD /usr/bin/zsh