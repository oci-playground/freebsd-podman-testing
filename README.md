# Podman testing on FreeBSD

## What is this project? 
The FreeBSD OCI Runtime Extension Working Group is running a time-boxed testing program for its experimental implementation of Podman on FreeBSD. 

## What is Podman on FreeBSD?
Simply put:
alias docker=podman.

Podman (Pod Manager) is a fully featured container engine that is a simple daemonless tool.  Podman provides a Docker-CLI comparable command line that eases the transition from other container engines and allows the management of pods, containers and images.

It’s a FreeBSD port of the https://github.com/containers stack. Install sysutils/podman-suite from FreeBSD Ports/Packages.

### Maturity
Suitable for testing and evaluation.

### CLI
Podman provides a CLI which is a drop-in replacement for docker.

### Storage
Container storage using the zfs and vfs storage drivers. ZFS is strongly preferred since its use of snapshots and clones makes it more efficient than vfs.

### Networking 
Supports docker-style networking using a port of https://github.com/containernetworking/plugins.

### Container Images
Container images use the same formats and infrastructure as containerd and can be shared between the two implementations. Podman uses Buildah internally to create container images.  Both tools share image (not container) storage, hence each can use or manipulate images (but not containers) created by the other.

