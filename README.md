# Cloud-Security-Toolbox

The MSS Cloud Security Toolbox image aims to distribute all tools needed for managing the automation and building process.

Tools include: 
 - Ansible (Latest pip version)
 - IBMCloud tools (Set through `IBMCLOUD_VERSION`, default `latest`)
    - Helm (`3.6.2`)
    - kubectl (via `Openshift`, see below)
 - Podman (Fedora repository maintained)
 - Docker CLI (Provided through Podman, not real docker)
 - Buildah (Fedora repository maintained)
 - Openshift Client (Origin) (`latest`)
 - ZSH (better default autocompletion)

The Cloud Security Toolbox is an OCI-Compatible image that is also compatible with Fedora's Toolbox project. 
It can run unprivileged with Podman. 

The default shell is ZSH, which allows for easier command completion, when used as a Toolbox.


## Using the toolbox with Docker/Podman

Podman is aiming to be a docker-compatible environment that runs in an unprivileged mode. 
It is available on all major Linux distrubutions and MacOSx.
When installing the podman-docker package, podman is exposed as the `docker` command, making it backwards compatible.

All commands in this section will use `docker`, but could be replaced with `podman`. 
For security reasons `podman` is preferred over docker on user workstations.

### Install
#### From Dockerfile
#### From image repository


## Using the toolbox with Fedora Toolbox

**Note** Fedora Toolbox will automatically mount your home directory to the toolbox home directory. 
This makes it an easy solution for local testing. If this is undesired, refer to Docker Usage. 

Toolbox is also available on OpenSuse and Ubuntu alledgedly, but best works on RPM-based systems.

### Install

#### From Dockerfile

#### From image repository